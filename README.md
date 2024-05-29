# Overview

People think they are getting smarter by using passphrases. Let's prove them wrong!

This project includes a massive Portuguese/Brazil oriented wordlist of phrases (2433732) and two hashcat rule files for GPU-based cracking. The rules will create over 1,000 permutations of each phase, resulting in over two and a half billion password permutations kept entirely on the brazillian context.

This project is adapted from [Initstring project](https://github.com/initstring/passphrase-wordlist). 

To use this project, you need:

- The wordlist `passphrases.txt`, which you can find under [releases](https://github.com/victormagalhaess/pt-br-passphrase-wordlist/releases).
- Both hashcat rules [here](/hashcat-rules/).

**WORDLIST LAST UPDATED**: May 2024

# Usage

Generally, you will use with hashcat's `-a 0` mode which takes a wordlist and allows rule files. It is important to use the rule files in the correct order, as rule #1 mostly handles capital letters and spaces, and rule #2 deals with permutations.

Here is an example for NTLMv2 hashes: If you use the `-O` option, watch out for what the maximum password length is set to - it may be too short.

```
hashcat -a 0 -m 5600 hashes.txt passphrases.txt -r passphrase-rule1.rule -r passphrase-rule2.rule -O -w 3
```

# Sources Used

Some sources are pulled from a static dataset, like a Kaggle upload. Others I generate myself using various scripts and APIs. I might one day automate that via CI, but for now you can see how I update the dynamic sources [here](/utilities/updating-sources.md).

| <ins>**source file name**</ins> | <ins>**source type**</ins> | <ins>**description**</ins> |
| --- | --- | --- |
| wiktionary-2024-05-29.txt | dynamic | Article titles scraped from Wiktionary's index dump [here.](https://dumps.wikimedia.org/ptwiktionary) |
| wikipedia-2024-05-29.txt | dynamic | Article titles scraped from the Wikipedia `pages-articles-multistream-index` dump generated 29-May-2024 [here.](https://dumps.wikimedia.org/ptwiki) |
| br-poi-2022-11-19.txt | dynamic | [BR POI dataset](https://download.geonames.org/export/dump/) using the 'BR' file from 29-May-2024. |
| books.txt | static | Kaggle dataset with titles from over 1000 brazillian books. |
| soap-operas.txt | static | Kaggle dataset with titles from brazillian soap-operas |
| football-teams.txt | static | Kaggle dataset with names from brazillian football teams |
| movies.txt | static | Kaggle dataset with names from brazillian football teams |

# Hashcat Rules

The rule files are designed to both "shape" the password and to mutate it. Shaping is based on the idea that human beings follow fairly predictable patterns when choosing a password, such as capitalising the first letter of each word and following the phrase with a number or special character. Mutations are also fairly predictable, such as replacing letters with visually-similar special characters.

Given the phrase `num sei só sei que foi assim` the first hashcat rule will output the following:

```
num sei só sei que foi assim
num-sei-só-sei-que-foi-assim
num.sei.só.sei.que.foi.assim
num_sei_só_sei_que_foi_assim
numseisóseiquefoiassim
Num Sei Só Sei Que Foi Assim
NUM SEI SÓ SEI QUE FOI ASSIM
nUM SEI SÓ SEI QUE FOI ASSIM
NumSeiSóSeiQueFoiAssim
nUMSEISÓSEIQUEFOIASSIM
NUMSEISÓSEIQUEFOIASSIM
Num Sei Só Sei Que Foi Assim
NumSeiSóSeiQueFoiAssim
Num-Sei-Só-Sei-Que-Foi-Assim
Num.Sei.Só.Sei.Que.Foi.Assim
Num_Sei_Só_Sei_Que_Foi_Assim
```

Adding in the second hashcat rule makes things get a bit more interesting. That will return a huge list per candidate. Here are a couple examples:

```
Nvm$3i$ó$3iQueF0i@ssim!
Nvm-Sei-Só-Sei-Que-Foi-Assim
numseisoSeiquefoiassim2020!
NVM SEI SÓ SEI QUE FOI ASSIM
```

# Additional Info

Optionally, some researchers might be interested in the script used to clean the raw sources into the wordlist [here](/utilities/cleanup.py). It is an adapted version of Initstring's script, found [here](https://github.com/initstring/passphrase-wordlist/blob/master/utilities/cleanup.py)

The cleanup script works like this:

```
$ python3 cleanup.py infile.txt outfile.txt
Reading from ./infile.txt: 505 MB
Wrote to ./outfile.txt: 250 MB
Elapsed time: 0:02:53.062531

```

Enjoy!
