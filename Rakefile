require 'csv'
require 'rest-client'

desc "Get country code for lat-longs. ENV: input, output, usernames, purpose"
task :geocode do
  contents = CSV.parse(File.read(ENV['input']))
  usernames = ENV["usernames"].split(",").collect {|username| username.strip}

  CSV.open(ENV["output"], "w+") do |csv|
    #NOTE We don't want to hit the geonames's 2k/hour request limit. So safely use 1900
    contents.each_slice(1900).with_index do |batch, index|
      batch.each do |row|
        #NOTE If header row, just add and skip
        if row[0] == "name"
          csv << (row + ["country_code"])
          next
        end

        begin
          params = {params: { lat: row[6], lng: row[7], username: usernames[index]}}
          result = RestClient.get("http://api.geonames.org/countryCode", params)

          #NOTE returned data is only the country code.
          # If anything more than that,
          # then it's junk html that explains something's wrong.
          if result.chomp.length > 3
            raise StandardError.new("Maybe error. Longer than usual result")
          end

          csv << (row + [result.chomp])
        rescue => e
          puts e.inspect
          puts "Row with ID #{row[5]} could not be geocoded"
        end
      end
    end
  end

  puts "-- Done ~! --"
end


desc "To check if the generated CSV works fine"
task :test_csv do
  CSV.parse File.read(ENV["input"])
end