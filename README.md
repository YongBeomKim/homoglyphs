# Homoglyphs

Homoglyphs -- python library for getting [homoglyphs](https://en.wikipedia.org/wiki/Homoglyph) and converting to ASCII.


## Features

It's like [confusable_homoglyphs](https://github.com/vhf/confusable_homoglyphs) but with some features:

* Load only needed alphabet to memory.
* Work as quick as possible.
* Converting to ASCII.
* Language management (detect language, get alphabet for language).
* Alphabet categories management (detect category, get alphabet for category).
* More configurable.
* More stable.


## Installation

```
sudo pip install homoglyphs
```


## Usage

Importing:

```python
import homoglyphs as hg
```

### Languages

```python
#detect
hg.Languages.detect('w')
# {'pl', 'da', 'nl', 'fi', 'cz', 'sr', 'pt', 'it', 'en', 'es', 'sk', 'de', 'fr', 'ro'}
hg.Languages.detect('т')
# {'mk', 'ru', 'be', 'bg', 'sr'}
hg.Languages.detect('.')
# set()

# get alphabet for languages
hg.Languages.get_alphabet(['ru'])
# {'в', 'Ё', 'К', 'Т', ..., 'Р', 'З', 'Э'}
```

### Categories

Categories -- ([aliases from ISO 15924](https://en.wikipedia.org/wiki/ISO_15924#List_of_codes)).

```python
#detect
hg.Categories.detect('w')
# 'LATIN'
hg.Categories.detect('т')
# 'CYRILLIC'
hg.Categories.detect('.')
# 'COMMON'

# get alphabet for categories
hg.Categories.get_alphabet(['CYRILLIC'])
# {'ӗ', 'Ԍ', 'Ґ', 'Я', ..., 'Э', 'ԕ', 'ӻ'}
```

### Homoglyphs

Get homoglyphs:

```python
# get latin combinations (by default initiated only latin alphabet)
hg.Homoglyphs().get_combinations('q')
# ['q', '𝐪', '𝑞', '𝒒', '𝓆', '𝓺', '𝔮', '𝕢', '𝖖', '𝗊', '𝗾', '𝘲', '𝙦', '𝚚']
```

Alphabet loading:

```python
# load alphabet on init by categories
homoglyphs = hg.Homoglyphs(categories=('LATIN', 'COMMON', 'CYRILLIC'))  # alphabet will be loaded here
homoglyphs.get_combinations('гы')
# ['rы', 'гы', 'ꭇы', 'ꭈы', '𝐫ы', '𝑟ы', '𝒓ы', '𝓇ы', '𝓻ы', '𝔯ы', '𝕣ы', '𝖗ы', '𝗋ы', '𝗿ы', '𝘳ы', '𝙧ы', '𝚛ы']

# load alphabet on init by languages
homoglyphs = hg.Homoglyphs(categories=None, languages={'ru', 'en'})  # alphabet will be loaded here
homoglyphs.get_combinations('гы')
# ['rы', 'гы']

# load alphabet by demand
homoglyphs = hg.Homoglyphs(categories=None, languages={'en'}, strategy=hg.STRATEGY_LOAD)
# ^ alphabet will be loaded here for "en" language
homoglyphs.get_combinations('гы')
# ^ alphabet will be loaded here for "ru" language
# ['rы', 'гы']
```

### Converting glyphs to ASCII chars

```python
homoglyphs = hg.Homoglyphs(categories=None, languages={'en'}, strategy=hg.STRATEGY_LOAD)

# convert
homoglyphs.to_ascii('тест')
# ['tect']
homoglyphs.to_ascii('ХР123.')  # this is cyrillic "х" and "р"
# ['XP123.', 'XPI23.', 'XPl23.']

# string with chars which can't be converted by default will be ignored
homoglyphs.to_ascii('лол')
# []

# you can set strategy for removing not converted non-ASCII chars from result
homoglyphs = hg.Homoglyphs(
    categories=None,
    languages={'en'},
    strategy=hg.STRATEGY_LOAD,
    ascii_strategy=hg.STRATEGY_REMOVE,
)
homoglyphs.to_ascii('лол')
# ['o']
```
