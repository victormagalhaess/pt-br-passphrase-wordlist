# Notes on updating sources

Sure, this should be a CI job. But hey, it's a start.

Some of the source files get regular updates. Below is a guide to obtaining those, preparing them for cleaning, actually cleaning, and then merging into the existing list.

## IBGE popular brazillian names 

```
curl "https://servicodados.ibge.gov.br/api/v1/censos/nomes/ranking?qtd=10000000" -o popular-names.json 
cat ./popular-names.json | jq -r '.[].nome' > ./ibge-names-$(date +%Y-%m-%d).txt
rm ./popular-names.json
```

## Brazillian holiday names

```
curl "https://brasilapi.com.br/api/feriados/v1/2024" -o holidays.json
cat ./holidays.json | jq -r '.[].name' > ./holiday-names-$(date +%Y-%m-%d).txt
rm ./holidays.json
```

## Portuguese Wikipedia article titles & category names

```
wget https://dumps.wikimedia.org/ptwiki/latest/ptwiki-latest-pages-articles-multistream-index.txt.bz2
bunzip2 ./ptwiki-latest-pages-articles-multistream-index.txt.bz2
cat ./ptwiki-latest-pages-articles-multistream-index.txt | cut -d: -f 3 > ./wikipedia-$(date +%Y-%m-%d).txt
rm ptwiki-latest-pages-articles-multistream-index.txt

```

## Portuguese Wiktionary titles

```
wget https://dumps.wikimedia.org/ptwiktionary/latest/ptwiktionary-latest-all-titles.gz
gunzip ptwiktionary-latest-all-titles.gz
cat ptwiktionary-latest-all-titles | awk -F '\t' '{print $2}' > ./wiktionary-$(date +%Y-%m-%d).txt
rm ptwiktionary-latest-all-titles

```

## BR POI dataset

```
wget http://download.geonames.org/export/dump/BR.zip
unzip ./BR.zip
cat BR.txt | awk -F '\t' '{print $3}' > ./BR-poi-$(date +%Y-%m-%d).txt
rm BR.zip
rm BR.txt
rm readme.txt
```

## Combining

With all raw files in the same folder:

```
cat ./data/*.txt | sort -u > raw.txt
python3 ./cleanup.py raw.txt passphrases.txt
```