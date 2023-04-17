To create a Ruby script that runs PDF info on all PDFs in a folder and writes the results into SQLite, you'll need the following:

* The pdfinfo tool from the Xpdf suite (https://www.xpdfreader.com/download.html) installed on your system.
* The SQLite3 gem for Ruby: gem install sqlite3

Here's the script:

```ruby
#!/usr/bin/env ruby

require 'sqlite3'
require 'open3'

def pdf_info(file)
  output, _ = Open3.capture2("pdfinfo", file)
  info = {}
  output.each_line do |line|
    key, value = line.split(':', 2)
    info[key.strip] = value.strip
  end
  info
end

def create_table(db)
  db.execute <<-SQL
    CREATE TABLE IF NOT EXISTS pdf_info (
      id INTEGER PRIMARY KEY AUTOINCREMENT,
      filename TEXT,
      title TEXT,
      author TEXT,
      creator TEXT,
      producer TEXT,
      creation_date TEXT,
      modification_date TEXT,
      pages INTEGER
    );
  SQL
end

def insert_pdf_info(db, info)
  query = <<-SQL
    INSERT INTO pdf_info (
      filename, title, author, creator, producer, creation_date, modification_date, pages
    ) VALUES (
      ?, ?, ?, ?, ?, ?, ?, ?
    );
  SQL
  db.execute query, info.values_at("File", "Title", "Author", "Creator", "Producer", "CreationDate", "ModDate", "Pages")
end

dir_path = ARGV[0] || '.'
db_path = ARGV[1] || 'pdf_info.sqlite3'

db = SQLite3::Database.new(db_path)
create_table(db)

Dir.glob(File.join(dir_path, '*.pdf')) do |pdf_path|
  begin
    info = pdf_info(pdf_path)
    insert_pdf_info(db, info.merge("File" => pdf_path))
    puts "Processed: #{pdf_path}"
  rescue => e
    puts "Error processing #{pdf_path}: #{e}"
  end
end

db.close
```

To run the script, save it to a file named pdf_info_to_sqlite.rb, and then run:

```bash
ruby pdf_info_to_sqlite.rb /path/to/folder_with_pdfs /path/to/sqlite_database.db
```

Replace /path/to/folder_with_pdfs with the path to the folder containing the PDFs and /path/to/sqlite_database.db with the path to the SQLite database you want to store the results in. If you don't provide these paths, the script will use the current directory for PDFs and create an SQLite database named pdf_info.sqlite3 in the current directory.


^ this was generated with GPT4 prompt:
* `create a ruby script that will run pdf info on all pdfs ina folder and write the results into sqlite` 
* and edited by a human to work
