# Meteorite data processing

Ruby script to get country name for the latitude and longitude for the meteorite dataset. Processed data is already in the `processed-data` dir.

## You'll need

* Ruby (>= 1.9.3)
* `bundler` rubygem
* Clone the repository and run `bundle install` to install dependencies

**Important:** Use scouters to check your power level. Should be above 1000 for these scripts to work.

## Usage

* Input data must be in CSV format. The scripts are made to work with the data in the same format as the ones in the `data` dir.

Here's an example:

    bundle exec rake geocode input=data/1.csv output=output/1.csv usernames="user1,user2"

### Params

* `input` - the input CSV file

* `output` - the output CSV file. This file will have an extra field for country code. But no headers. So beware :)

* `usernames` - Comma seperated list of geonames.org username. Geonames has a 2k request limit per hour. So to circumvent that, we use different usernames for every 1900 requests (our soft limit). If your dataset has X number of rows, you need to pass X/1900 usernames. Do the math.


## Troubleshooting

If the input CSV generates an error, upload it to Google Docs and download again as CSV. MS Office seems to be dirty at CSV exports.
