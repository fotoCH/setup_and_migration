require 'data-import'

# work in progress
class Importer

  def self.run!
    result = []
    index = 0
    count = 0
    prev = nil
    db = Sequel.connect(adapter: 'mysql2', socket: '/Applications/MAMP/tmp/mysql/mysql.sock', user: 'root', password: 'root', database: 'foto-ch')
    db.fetch("SELECT id, fotografen_id, vorname, nachname FROM namen ORDER BY fotografen_id") do |row|
      if row[:fotografen_id] == prev
        result[index][:"alt_label_#{count}"] = "#{row[:vorname]}, #{row[:nachname]}"
        count = count + 1
      else
        prev = row[:fotografen_id]
        count = 0
      end
      p row
    end
  end

end


desc "Imports the date from the source database"
task :import do
  Importer.run!
end
