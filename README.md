
# countrycode <img src="https://user-images.githubusercontent.com/987057/167296405-e7798ac8-03e7-444e-acaf-d99fc42d1c9e.png" align="right" alt="" width="120" />

<!-- badges: start -->

[![DOI](http://joss.theoj.org/papers/10.21105/joss.00848/status.svg)](https://doi.org/10.21105/joss.00848)
[![AppVeyor build
status](https://ci.appveyor.com/api/projects/status/github/vincentarelbundock/countrycode?branch=master&svg=true)](https://ci.appveyor.com/project/vincentarelbundock/countrycode)
[![R build
status](https://github.com/vincentarelbundock/countrycode/workflows/R-CMD-check/badge.svg)](https://github.com/vincentarelbundock/countrycode/actions)
![CRAN
downloads](http://cranlogs.r-pkg.org/badges/grand-total/countrycode)
<!-- badges: end -->

`countrycode` standardizes country names, converts them into \~40
different coding schemes, and assigns region descriptors. Scroll down
for more details or visit the [countrycode CRAN
page](http://cran.r-project.org/web/packages/countrycode/index.html)

If you use `countrycode` in your research, we would be very grateful if
you could cite our paper:

> Arel-Bundock, Vincent, Nils Enevoldsen, and CJ Yetman, (2018).
> countrycode: An R package to convert country names and country codes.
> Journal of Open Source Software, 3(28), 848,
> <https://doi.org/10.21105/joss.00848>

# Table of Contents

  - [Why
    `countrycode`?](https://vincentarelbundock.github.io/countrycode#why-countrycode)
  - [Installation](https://vincentarelbundock.github.io/countrycode#installation)
  - [Supported
    codes](https://vincentarelbundock.github.io/countrycode#why-countrycode)
  - [`countrycode`](https://vincentarelbundock.github.io/countrycode#countrycode)
      - [Convert of a single name or
        code](https://vincentarelbundock.github.io/countrycode#convert-a-single-name-or-code)
      - [Vectors and
        data.frames](https://vincentarelbundock.github.io/countrycode#vectors-and-data.frames)
      - [Flags](https://vincentarelbundock.github.io/countrycode#flags)
      - [Country names in 600+ different languages and
        formats](https://vincentarelbundock.github.io/countrycode#country-names-in-600-different-languages-and-formats)
      - [`custom_dict` and `get_dictionary()`: Custom dictionaries and
        cross-walks](https://vincentarelbundock.github.io/countrycode/#custom-dictionaries-and-cross-walks-get_dictionary-and-custom_dict)
      - [`destination`: Fallback
        codes](https://vincentarelbundock.github.io/countrycode#destination-fallback-codes)
      - [`nomatch`: Fill in missing codes
        manually](https://vincentarelbundock.github.io/countrycode#nomatch-fill-in-missing-codes-manually)
      - [`custom_match`: Override default
        values](https://vincentarelbundock.github.io/countrycode#custom_match-override-default-values)
      - [`warn`: Silence
        warnings](https://vincentarelbundock.github.io/countrycode#warn-silence-warnings)
  - [`countryname`: Convert country names from any
    language](https://vincentarelbundock.github.io/countrycode#countryname-convert-country-names-from-any-language)
  - [Custom conversion functions and
    “crosswalks”](https://vincentarelbundock.github.io/countrycode#custom-conversion-functions-and-crosswalks)
  - [Contributions](https://vincentarelbundock.github.io/countrycode#contributions)

# Why `countrycode`?

### The Problem

Different data sources use different coding schemes to represent
countries (e.g. CoW or ISO). This poses two main problems: (1) some of
these coding schemes are less than intuitive, and (2) merging these data
requires converting from one coding scheme to another, or from long
country names to a coding scheme.

### The Solution

The `countrycode` function can convert to and from 40+ different country
coding schemes, and to 600+ variants of country names in different
languages and formats. It uses regular expressions to convert long
country names (e.g. Sri Lanka) into any of those coding schemes or
country names. It can create new variables with various regional
groupings.

# Installation

From the R console, type:

``` r
install.packages("countrycode")
```

To install the latest development version, you can use the `remotes`
package:

``` r
library(remotes)
install_github('vincentarelbundock/countrycode')
```

# Supported codes

To get an up-to-date list of supported country codes, install the
package and type `?codelist`. These include:

  - 600+ variants of country names in different languages and formats.
  - AR5
  - Continent and region identifiers.
  - Correlates of War (numeric and character)
  - European Central Bank
  - [EUROCONTROL](https://www.eurocontrol.int) - The European
    Organisation for the Safety of Air Navigation
  - Eurostat
  - Federal Information Processing Standard (FIPS)
  - Food and Agriculture Organization of the United Nations
  - Global Administrative Unit Layers (GAUL)
  - Geopolitical Entities, Names and Codes (GENC)
  - Gleditsch & Ward (numeric and character)
  - International Civil Aviation Organization
  - International Monetary Fund
  - International Olympic Committee
  - ISO (2/3-character and numeric)
  - Polity IV
  - United Nations
  - United Nations Procurement Division
  - Varieties of Democracy
  - World Bank
  - World Values Survey
  - Unicode symbols (flags)

# `countrycode`

## Convert a single name or code

Load library:

``` r
library(countrycode)
```

Convert single country codes:

``` r
# ISO to Correlates of War
countrycode('DZA', origin = 'iso3c', destination = 'cown') 
```

    ## [1] 615

``` r
# English to ISO
countrycode('Albania', origin = 'country.name', destination = 'iso3c') 
```

    ## [1] "ALB"

``` r
# German or Italian to Arabic
countrycode(c('Algerien', 'Albanien'), origin = 'country.name.de', destination = 'un.name.ar') 
```

    ## [1] "الجزائر" "ألبانيا"

``` r
countrycode(c('Moldavia', 'Stati Uniti'), origin = 'country.name.it', destination = 'un.name.ar') 
```

    ## [1] "جمهورية مولدوفا"            "الولايات المتحدة الأمريكية"

## Convert a vector of country codes

``` r
cowcodes <- c("ALG", "ALB", "UKG", "CAN", "USA")
countrycode(cowcodes, origin = "cowc", destination = "iso3c")
```

    ## [1] "DZA" "ALB" "GBR" "CAN" "USA"

Generate vectors and 2 data frames without a common id (i.e. can’t merge
the 2 df):

``` r
isocodes <- c(12,8,826,124,840)
var1     <- sample(1:500,5)
var2     <- sample(1:500,5)
df1      <- data.frame(cowcodes,var1)
df2      <- data.frame(isocodes,var2)
```

Inspect the data:

``` r
df1
```

    ##   cowcodes var1
    ## 1      ALG   22
    ## 2      ALB  159
    ## 3      UKG  100
    ## 4      CAN  447
    ## 5      USA  373

``` r
df2
```

    ##   isocodes var2
    ## 1       12   15
    ## 2        8  207
    ## 3      826  111
    ## 4      124  418
    ## 5      840  331

Create a common variable with the iso3c code in each data frame, merge
the data, and create a country identifier:

``` r
df1$iso3c   <- countrycode(df1$cowcodes, origin = "cowc", destination = "iso3c")
df2$iso3c   <- countrycode(df2$isocodes, origin = "iso3n", destination = "iso3c")
df3         <- merge(df1,df2,id="iso3c")
df3$country <- countrycode(df3$iso3c, origin = "iso3c", destination = "country.name")
df3
```

    ##   iso3c cowcodes var1 isocodes var2        country
    ## 1   ALB      ALB  159        8  207        Albania
    ## 2   CAN      CAN  447      124  418         Canada
    ## 3   DZA      ALG   22       12   15        Algeria
    ## 4   GBR      UKG  100      826  111 United Kingdom
    ## 5   USA      USA  373      840  331  United States

## Flags

`countrycode` can convert country names and codes to unicode flags. For
example, we can use the `gt` package to draw a table with countries and
their corresponding flags:

``` r
library(gt)
library(countrycode)

Countries <- c('Canada', 'Germany', 'Thailand', 'Algeria', 'Eritrea')
Flags <- countrycode(Countries, 'country.name', 'unicode.symbol')
dat <- data.frame(Countries, Flags)
gt(dat)
```

![gt\_flags](https://github.com/vincentarelbundock/countrycode/assets/987057/c0c29aa3-0aab-4274-af80-1ae3efc89203)

Note that embedding unicode characters in `R` graphics is possible, but
it can be tricky. If your output looks like `\U0001f1e6\U0001f1f6`, then
you could try feeding it to this function: `utf8::utf8_print()`. That
should cover a lot of cases without dipping into the complexity of
graphics devices. As a rule of thumb, if your output looks like `□□□□`
(boxes), things tend to get more complicated. In that case, you’ll have
to think about different output devices, file viewers, and/or file
formats (e.g., ‘SVG’ or ‘HTML’).

Since inserting unicode symbols into `R` graphics is not a
`countrycode`-specific issue, we won’t be able to offer any more support
than this. Good luck\!

## Country names in 600+ different languages and formats

The Unicode organisation hosts the CLDR project, which publishes many
variants of country names. For each language/culture locale, there is a
full set of names, plus possible ‘alt-short’ or ‘alt-variant’ variations
of specific country names.

``` r
countrycode('United States of America', origin = 'country.name', destination = 'cldr.name.en')
```

    ## [1] "United States"

``` r
countrycode('United States of America', origin = 'country.name', destination = 'cldr.short.en')
```

    ## [1] "US"

To see a full list of country name variants available, inspect this
data.frame:

``` r
head(countrycode::cldr_examples)
```

    ##              Code                    Example
    ## 1   cldr.name.agq                         TF
    ## 2    cldr.name.ak                         TF
    ## 3    cldr.name.am           የፈረንሳይ ደቡባዊ ግዛቶች
    ## 4    cldr.name.ar الأقاليم الجنوبية الفرنسية
    ## 5 cldr.name.ar_ly الأقاليم الجنوبية الفرنسية
    ## 6 cldr.name.ar_sa الأقاليم الجنوبية الفرنسية

## Custom dictionaries and cross-walks: `get_dictionary()` and `custom_dict`

The `custom_dict` argument accepts data frame which can be used as
custom dictionaries to create “crosswalks” between arbitrary entities
(non-countries). You can create your own dictionaries (see examples
below) or use one of the dictionaries already hosted on the
`countrycode` Github repository. The current list of available
dictionaries can be seen by calling:

``` r
get_dictionary()
```

    ## Available dictionaries: ch_cantons, exiobase3, global_burden_of_disease, gtap10, us_states

You can download a dictionary and see available fields with:

``` r
cd <- get_dictionary("us_states")
head(cd)
```

    ##   state.name state.abb    state.regex
    ## 1    Alabama        AL    .*alabama.*
    ## 2     Alaska        AK     .*alaska.*
    ## 3    Arizona        AZ    .*arizona.*
    ## 4   Arkansas        AR   .*arkansas.*
    ## 5 California        CA .*california.*
    ## 6   Colorado        CO   .*colorado.*

Now we can use the dictionary for conversions:

``` r
st <- c("Arkansas", "Quebec", "Tennessee")
countrycode(st, "state.regex", "state.abb", custom_dict = cd)
```

    ## Warning: Some values were not matched unambiguously: Quebec

    ## [1] "AR" NA   "TN"

``` r
countrycode(c("MN", "MA", "MO"), "state.abb", "state.name", custom_dict = cd)
```

    ## [1] "Minnesota"     "Massachusetts" "Missouri"

Here’s an example with the GTAP dataset:

``` r
cd <- get_dictionary("gtap10")
countrycode("Christmas Island", "country.name.en.regex", "gtap.cha", custom_dict = cd)
```

    ## [1] "AUS"

### `custom_dict`: the `ISOcodes` package

`countrycode` already supports ISO4217 (currencies) and ISO3166 (country
codes). The `ISOcodes` package supplies other codes, including ISO15924
(language writing systems), ISO639 (languages), and ISO8859 (computer
character encodings). Users can convert those codes using
`countrycode`’s `custom_dict` argument.

For example, the `ISOcodes::ISO_639_2` dataframe includes 4 columns:
`Alpha_3_B`, `Alpha_3_T`, `Alpha_2`, and `Name`. We can convert language
names like this:

``` r
countrycode('abk', 'Alpha_3_B', 'Name', custom_dict = ISOcodes::ISO_639_2)
```

    ## [1] "Abkhazian"

The `ISOcodes::ISO_8859` dataset is a 3-dimensional array where the
second dimension represents the character encoding. We take the subset
of `ISO_8859_1` codes and convert the dict to a dataframe for use in
`countrycode`’s `custom_dict` argument:

``` r
library(ISOcodes)
dict <- ISOcodes::ISO_8859[, 'ISO_8859_1', ]
dict <- data.frame(dict)
```

The resulting dataframe has 3 columns: `Code`, `Name`, `Character`. We
convert the code `0x00fd` like this:

``` r
countrycode("0x00fd", "Code", "Name", custom_dict = dict)
```

    ## [1] "LATIN SMALL LETTER Y WITH ACUTE"

``` r
countrycode("0x00fd", "Code", "Character", custom_dict = dict)
```

    ## [1] "ý"

## `destination`: Fallback codes

Some destination codes not cover all the relevant countries. For
example, “SRB” is included in the `iso3c` code but *not* in the `cowc`
code. Some users may want to use `cowc` but to fill in missing entries
with `iso3c` codes. We can do this by feeding a vector of code names to
the `destination` argument. `countrycode` will then try one after the
other.

For example,

``` r
x <- c("Algeria", "Serbia")

countrycode(x, "country.name", "cowc")
```

    ## Warning: Some values were not matched unambiguously: Serbia

    ## [1] "ALG" NA

``` r
countrycode(x, "country.name", "iso3c")
```

    ## [1] "DZA" "SRB"

``` r
countrycode(x, "country.name", c("cowc", "iso3c"))
```

    ## Warning: Some values were not matched unambiguously: Serbia

    ## [1] "ALG" "SRB"

## `nomatch`: Fill in missing codes manually

Use the `nomatch` argument to specify the value that `countrycode`
inserts where no match was found:

``` r
countrycode(c('DZA', 'USA', '???'), origin = 'iso3c', destination = 'country.name', nomatch = 'BAD CODE')
```

    ## [1] "Algeria"       "United States" "BAD CODE"

``` r
countrycode(c('Canada', 'Fake country'), origin = 'country.name', destination = 'iso3c', nomatch = 'BAD')
```

    ## [1] "CAN" "BAD"

## `custom_match`: Override default values

`countrycode` accepts a user supplied named vector of custom matches via
the `custom_match` argument. Any match pairs in the `custom_match`
vector will supercede the default results of the command. This allows
the user to convert to an available country code and make minor
post-edits all at once. The names of the named vector are used as the
origin code, and the values of the named vector are used as the
destination code.

For example, Eurostat uses a modified version of iso2c, with Greece (EL
instead of GR) and the UK (UK instead of GB) being the only differences.
Getting a proper result converting to Eurostat is easy to achieve using
the `iso2c` destination and the new `custom_match` argument. (Note:
since version 0.19, `countrycode` also includes a `eurostat`
origin/destination code, so while this is a good example, doing so for
Eurostat is not necessary)

Example: convert from country name to Eurostat code

``` r
library(countrycode)
country_names <- c('Greece', 'United Kingdom', 'Germany', 'France')
custom_match <- c(Greece = 'EL', `United Kingdom` = 'UK')
countrycode(country_names, 
            origin = 'country.name', 
            destination = 'iso2c', 
            custom_match = custom_match)
```

    ## [1] "EL" "UK" "DE" "FR"

Example: convert from Eurostat code to country name

``` r
library(eurostat)
library(countrycode)
df <- eurostat::get_eurostat("nama_10_lp_ulc")
custom_match <- c(EL = 'Greece', UK = 'United Kingdom')
countrycode(df$geo, origin = 'iso2c', destination = 'country.name', custom_match = custom_match) |>
    head()
```

    ## Warning: Some values were not matched unambiguously: EA, EA12, EA19, EA20, EU15, EU27_2020, EU28, XK

    ## [1] "Austria"  "Belgium"  "Bulgaria" "Cyprus"   "Czechia"  "Germany"

## `warn`: Silence warnings

Use `warn = TRUE` to print out a list of source elements for which no
match was found. When the source vector are long country names that need
to be matched using regular expressions, there is always a risk that
multiple regex will match a given string. When this is the case,
`countrycode` assigns a value arbitrarily, but the `warn` argument
allows the user to print a list of all strings that were matched many
times.

# `countryname`: Convert country names from any language

The function `countryname` tries to convert country names from any
language. For example:

``` r
library(countrycode)
x <- c('ジンバブエ', 'Afeganistãu',  'Barbadas', 'Sverige', 'UK',  
       'il-Georgia tan-Nofsinhar u l-Gżejjer Sandwich tan-Nofsinhar')

countryname(x)
```

    ## [1] "Zimbabwe"                              
    ## [2] "Afghanistan"                           
    ## [3] "Barbados"                              
    ## [4] "Sweden"                                
    ## [5] "UK"                                    
    ## [6] "South Georgia & South Sandwich Islands"

``` r
countryname(x, 'iso3c')
```

    ## [1] "ZWE" "AFG" "BRB" "SWE" "GBR" "SGS"

# Custom conversion functions

It is easy to to create alternative functions with different default
arguments and/or dictionaries. For example, we can create:

  - `name_to_iso3c` function that sets new defaults for the `origin` and
    `destination` arguments, and automatically converts country names to
    iso3c
  - `statecode` function to convert US state codes using a custom
    dictionary by default, that we download from the internet.

<!-- end list -->

``` r
#################################
#  new function: name_to_iso3c  #
#################################

# Custom defaults
name_to_iso3c <- function(sourcevar,
                          origin = "country.name",
                          destination = "iso3c",
                          ...) {
  countrycode(sourcevar, origin = origin, destination = destination, ...)
}

name_to_iso3c(c("Algeria", "Canada"))
```

    ## [1] "DZA" "CAN"

``` r
#############################
#  new function: statecode  #
#############################

# Download dictionary
state_dict <- "https://raw.githubusercontent.com/vincentarelbundock/countrycode/main/data/custom_dictionaries/data_us_states.csv"
state_dict <- read.csv(state_dict)

# Identify regular expression origin codes
attr(state_dict, "origin_regex") <- "state.regex"

# Define a custom conversion function
statecode <- function(sourcevar,
                      origin = "state.regex",
                      destination = "abbreviation",
                      custom_dict = state_dict,
                      ...) {
    countrycode(sourcevar,
                origin = origin,
                destination = destination,
                custom_dict = custom_dict,
                ...)
}

# Voilà!
x <- c("Alabama", "New Mexico")
statecode(x, "state.regex", "abbreviation")
```

    ## [1] "AL" "NM"

``` r
x <- c("AL", "NM", "VT")
statecode(x, "abbreviation", "state")
```

    ## [1] "Alabama"    "New Mexico" "Vermont"

# Contributions

## Adding a new code

New country codes are created by two files:

1.  `dictionary/get_*.R` is an `R` script which can scrape the code from
    an original online source (e.g., `get_world_bank.R`). This scripts
    only side effect is that it writes a CSV file to the `dictionary`
    folder.
2.  `dictionary/data_*.csv` is a CSV file with 1 column called
    `country`, which includes the English country name, and 1 or more
    columns named after the codes you want to add (e.g., `iso3c`,
    `un.name.en`, `continent`).

After creating those two files, you should:

  - Run `dictionary/build.R`
  - If the code is a valid origin code (i.e., no two countries share the
    same code), add it to the `valid_origin` vector in `R/countrycode.R`
  - Add the new code name to the documentation in `R/codelist.R`
  - Build the documentation using the devtools package:
    `devtools::document()`
  - Add a bullet point to `NEWS.md` file.

If you need help with any of these steps, or if you just want to submit
a CSV file, feel free to open an issue on Github or write an email to
Vincent. I’ll be happy to help you out\!

## Custom dictionaries

The `countrycode` repository holds several custom dictionaries:
<https://github.com/vincentarelbundock/countrycode/tree/master/data/custom_dictionaries>

To add your own custom dictionary, please make sure that:

1.  You save a comma-separated CSV file that looks something like
    data/custom\_dictionaries/us\_states.csv
2.  The custom dictionary has a unique purpose (not overlapping with
    existing custom dictionaries)
3.  It uses UTF-8 encoding and conforms to RFC 4180 CSV standard
    (e.g. comma-delimited, etc.).
      - `R` commands to produce such a file are shown below.
4.  <NA>/blank fields are blank, not the string ‘NA’ (not RFC 4180, but
    important here because of Namibia)
5.  It has concise, sensible, valid (in the R data frame sense) column
    header names

Using base write.csv:

``` r
write.csv(custom_dict, 'custom_dict.csv', quote = TRUE, na = '', 
          row.names = FALSE, qmethod = 'double', fileEncoding = 'UTF-8')
```

Using `readr`:

``` r
readr::write_csv(custom_dict, 'custom_dict.csv', na = '')
```

## Custom dictionary attributes

When using custom dictionaries, it is often useful to give “meta”
information to `countrycode` so that it knows how to use certain codes.
To do this, we can set attributes of the dictionary. In this example, we
download a dictionary of US state codes. Then, we identify a column of
regular expressions using the `origin_regex` attribute, and we identify
the valid origin codes using the `origin_valid` attribute.

``` r
state_dict <- "https://raw.githubusercontent.com/vincentarelbundock/countrycode/main/data/custom_dictionaries/data_us_states.csv"
state_dict <- read.csv(state_dict)

attr(state_dict, "origin_regex") <- "state.regex"
attr(state_dict, "origin_valid") <- c("state.regex", "abbreviation")

countrycode("Alabama", "state.regex", "abbreviation", custom_dict = state_dict)
```

    ## [1] "AL"

``` r
countrycode("AL", "abbreviation", "state", custom_dict = state_dict)
```

    ## [1] "Alabama"

``` r
countrycode("Alabama", "state", "abbreviation", custom_dict = state_dict)
```

    ## Error in countrycode("Alabama", "state", "abbreviation", custom_dict = state_dict): The `origin` argument must be a string of length 1 equal to one of these values: state.regex, abbreviation.
