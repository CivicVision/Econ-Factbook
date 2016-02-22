dot:= .
underscore:= _
DATE= 2014:2014
WORLDBANK_API = http://api.worldbank.org/countries/all/indicators/
INDICATORS = NY.GDP.PCAP.CD SP.POP.TOTL NY.GDP.PCAP.PP.CD NY.GDP.PCAP.KD.ZG

GENERATED_FILES = $(INDICATORS:=.csv)

.PHONY: all

all: $(GENERATED_FILES)

.INTERMEDIATE: %_api.json
%_api.json:
	curl -o $@ "$(WORLDBANK_API)$(@:_api.json=)?date=$(DATE)&per_page=20000&format=json"

.INTERMEDIATE: %_api_formatted.json
%_api_formatted.json: %_api.json
	cat $< | jq '[.[1] | .[] | { country_code: .country.id, country_name: .country.value, $(subst $(dot),$(underscore),$(@:_api_formatted.json=)): .value, date: .date }]' > $@

$(GENERATED_FILES): %.csv : %_api_formatted.json
	in2csv $< > data/$@
