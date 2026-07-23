# MajinBook

This document outlines the structure and metadata schemas of the datasets released with *MajinBook: An open catalogue of digitally mediated world literature* by [Antoine Mazières](https://mazieres.gitlab.io/) and [Thierry Poibeau](https://www.lattice.cnrs.fr/membres/chercheurs-ou-enseignants-chercheurs/thierry-poibeau/).

**Abstract:**
*This data paper introduces MajinBook, an open catalogue designed to facilitate the use of shadow libraries, such as Library Genesis and Z-Library, for computational social science and cultural analytics. By linking metadata from these vast, crowd-sourced archives with structured bibliographic data from Goodreads, we create a high-precision corpus of over 539,000 references to digitally mediated English-language books. Spanning three centuries and reflecting a contemporary selection bias, these entries are enriched with first publication dates, genres, and popularity metrics such as ratings and reviews. Our methodology prioritises natively digital EPUB files to ensure machine-readable quality, while addressing biases in traditional corpora such as HathiTrust, and includes secondary datasets for French-, German-, and Spanish-language works. We evaluate the linkage strategy for accuracy, release all underlying data openly, and discuss the project’s legal permissibility under EU and U.S. frameworks for text and data mining in research.*

The data is available on [Zenodo](https://doi.org/10.5281/zenodo.17609566) and [HuggingFace](https://huggingface.co/datasets/mazieres/majinbook).

The paper is on [ArXiv](https://arxiv.org/abs/2511.11412).

All files are in the [JSON Lines text file format](https://jsonlines.org/).

## 1\. The MajinBook's Catalogue

This section describes the primary high-precision English catalogue introduced in the [paper](https://arxiv.org/abs/2511.11412). The secondary datasets in French, German and Spanish follow the same model.

**Files**

- Primary dataset:  [`majinbook_eng.jsonl`](https://zenodo.org/records/17609567/files/majinbook_eng.jsonl.gz?download=1) (English) — `539,530` lines.
- Secondary datasets:
  * [`majinbook_fra.jsonl`](https://zenodo.org/records/17609567/files/majinbook_fra.jsonl.gz?download=1) (French) — `47,960` lines.
  * [`majinbook_deu.jsonl`](https://zenodo.org/records/17609567/files/majinbook_deu.jsonl.gz?download=1) (German) — `35,559` lines.
  * [`majinbook_spa.jsonl`](https://zenodo.org/records/17609567/files/majinbook_spa.jsonl.gz?download=1) (Spanish) — `30,169` lines.


**JSON Record Example**

```json
{
  "first_pub_year": 1913,
  "authors": [
    [
      233619,
      "Marcel Proust"
    ]
  ],
  "genres": [
    "Classics",
    "Literature",
    "Philosophy",
    "20th Century",
    "Novels",
    "Fiction",
    "Literary Fiction",
    "Classic Literature",
    "France",
    "French Literature"
  ],
  "n_reviews": 356,
  "n_ratings": 3635,
  "rating": 4.28,
  "title": "Remembrance of Things Past: Volume I - Swann's Way & Within a Budding Grove",
  "work_id": 45683795,
  "zlibrary_ids": [
    11588490
  ],
  "libgen_ids": null
}
```

**Schema Description**

| Field | Type | Coverage<sup>1</sup> | Description |
| --- | --- | --- | --- |
| `first_pub_year` | Integer | 100% | The work's first publication year; Range: 1456-2024 | 
| `authors` | List[int, str] | 100% | A list of the work's authors; each entry contains `[Goodreads Author ID (int), Author's Full Name (str)]`<sup>2</sup> |
| `genres` | List[str] | 84% | A list of genres (`str`) associated with the work; Max length is 10 |
| `n_reviews` | Integer | 99% | Count of reviews on Goodreads |
| `n_ratings` | Integer | 100% | Count of ratings on Goodreads |
| `rating` | Float | 100% | Average rating (aggregated across all editions); Range: 0.00–5.00 |
| `title` | String | 100% | The work's title in the catalogue's language |
| `work_id` | Integer | 100% | Unique Goodreads Work ID<sup>3</sup> |
| `zlibrary_ids` | List[Integer] | 93%<sup>4</sup> | List of Z-Library IDs (`int`) corresponding to this work |
| `libgen_ids` | List[str] | 60%<sup>4</sup> | List of LibGen IDs (`str`) corresponding to this work |

Notes:
1. Coverage is for the primary dataset only (English). Coverage varies for secondary datasets regarding `genres` and `n_reviews`, see the [paper](https://arxiv.org/abs/2511.11412).
2. `Goodreads Author ID (int)` corresponds to the ID of the Author found in the URL of their profile, as in [`goodreads.com/author/show/233619`](https://www.goodreads.com/author/show/233619)
3. Goodreads Work ID, as in [`goodreads.com/work/editions/45683795`](https://www.goodreads.com/work/editions/45683795)
4. `zlibrary_ids` and `libgen_ids` cannot both be `null`.

<br />

## 2\. Underlying datasets

This section describes the metadata datasets collected and processed to construct the MajinBook catalogue.

### 2\.1 Shadow Library metadata

#### Z-Library

**File**: [`zlibrary.jsonl`](https://zenodo.org/records/17609567/files/zlibrary.jsonl.gz?download=1) — `8,097,488` lines

**JSON Record Example**

```json
{
  "id": 11588490,
  "title": "Remembrance of Things Past, Volume I",
  "authors": "Marcel Proust",
  "pub_year": 2011,
  "lang_iso": "eng",
  "publishers": "Knopf Doubleday Publishing Group",
  "isbns": [
    "9780307808554"
  ]
}
```

**Schema Description**

 | Field | Type | Coverage | Description |
| --- | --- | --- | --- |
| `id` | Integer | 100% | Unique Z-Library ID |
| `title` | String | 100% | The title of the book |
| `authors` | String | 99% | Unformatted string of author names |
| `pub_year` | Integer | 81% | Publication year |
| `lang_iso` | String | 66% | ISO 639-3 code of the book's language |
| `publishers` | String | 65% | Unformatted string of publishers |
| `isbns` | List[str] | 29% | List of ISBN-13 identifiers |

<br />

#### Library Genesis (LibGen)

**File**: [`libgen.jsonl`](https://zenodo.org/records/17609567/files/libgen.jsonl.gz?download=1) — `3,032,730` lines

**JSON Record Example**

```json
{
  "id": "bc0d6411ddd67b2c2d43b39bc8472cfc",
  "collection": "fiction",
  "title": "Alice's Adventures in Wonderland and What the Tortoise Said to Achilles and Other Riddles",
  "lang_iso": "eng",
  "authors": [
    "Lewis Carroll"
  ],
  "pub_year": 2012,
  "publisher": "West Margin Press",
  "isbns": [
    "9780882408712"
  ],
  "asin": "B007HOO22Y"
}
```

**Schema Description**

 | Field | Type | Coverage | Description |
| --- | --- | --- | --- |
| `id` | String | 100% | Unique Library Genesis ID; an MD5 hash |
| `collection` | String | 100% | LibGen's collection of origin, either `fiction` or `nonfiction` |
| `lang_iso` | String | 99% | ISO 639-3 code of the book's language |
| `title` | String | 99% | The title of the book |
| `authors` | List[str] | 99% | List of author names |
| `pub_year` | Integer | 83% | Publication year |
| `publisher` | String | 78% | The book's publisher |
| `isbns` | List[str] | 57% | List of ISBN-13 identifiers |
| `asin` | String | 18% | Amazon Standard Identification Number (starts with 'B') |

<br />

### Goodreads

#### Goodreads Works

**File**: [`goodreads_works.jsonl`](https://zenodo.org/records/17609567/files/goodreads_works.jsonl.gz?download=1) — `4,778,124` lines

**JSON Record Example**

```json
{
  "id": 55548884,
  "suggestions": [
    2998,
    5326,
    5659,
    ...
  ],
  "first_pub_year": 1865,
  "n_ratings": 418248,
  "rating": 3.99,
  "main_authors": [
    8164
  ]
}
```

**Schema Description**

| Field | Type | Coverage | Description |
| --- | --- | --- | --- |
| `id` | Integer | 100% | Unique Goodreads Work ID (as in [`goodreads.com/work/editions/55548884`](https://www.goodreads.com/work/editions/55548884)) |
| `suggestions` | List[int] | 39% | List of suggested Goodreads Edition IDs (found in [`goodreads.com/book/similar/55548884`](https://www.goodreads.com/book/similar/55548884)) |
| `first_pub_year` | Integer | 61% | The work's first publication year |
| `n_ratings` | Integer | 99% | Aggregated count of ratings across all editions of the work |
| `rating` | Float | 99% | Aggregated average rating across all editions of the work |
| `main_authors` | List[int] | 100% | List of Author IDs appearing most frequently across all editions |

<br />

#### Goodreads Editions

**File**: [`goodreads_editions.jsonl`](https://zenodo.org/records/17609567/files/goodreads_editions.jsonl.gz?download=1) — `28,105,913` lines

**JSON Record Example**

```json
{
  "id": 60671823,
  "work_id": 55548884,
  "title": "Alice's Adventures in Wonderland (Hardcover)",
  "authors": [
    8164,
    59749
  ],
  "pub_year": null,
  "lang": "English",
  "lang_iso": "eng",
  "publisher": "Pan Macmillan",
  "isbn": "9781529002461",
  "asin": null,
  "rating": 4.08,
  "n_ratings": 71699
}
```

**Schema Description**

| Field | Type | Coverage | Description |
| --- | --- | --- | --- |
| `id` | Integer | 100% | Unique Goodreads Edition ID (as in [`goodreads.com/book/show/60671823`](https://www.goodreads.com/book/show/60671823)) |
| `work_id` | Integer | 100% | The Work ID to which the edition belongs |
| `title` | String | 99% | The edition's title |
| `authors` | List[int] | 99% | List of Goodreads Author IDs |
| `pub_year` | Integer | 89% | The edition's publication year |
| `lang` | String | 86% | The edition's language name |
| `lang_iso` | String | 86% | ISO 639-3 language code |
| `publisher` | String | 92% | The publisher's name |
| `isbn` | String | 62% | The edition's ISBN-13 identifier |
| `asin` | String | 49% | The edition's ASIN (starts with 'B') |
| `rating` | Float | 46% | Average rating for this specific edition |
| `n_ratings` | Integer | 46% | Count of ratings for this specific edition |

<br />

#### Goodreads Authors

**File**: [`goodreads_authors.jsonl`](https://zenodo.org/records/17609567/files/goodreads_authors.jsonl.gz?download=1) — `2,150,522` lines

**JSON Record Example**

```json
{
  "id": 8164,
  "full_name": "Lewis Carroll",
  "birth_place": "Daresbury, Cheshire, England, The United Kingdom",
  "birth_year": 1832,
  "death_year": 1898,
  "works": [
    87631877,
    164806665,
    155893772,
    ...
  ]
}
```

**Schema Description**

| Field | Type | Coverage | Description |
| --- | --- | --- | --- |
| `id` | Integer | 100% | Unique Goodreads Author ID (as in [`goodreads.com/author/show/8164`](https://www.goodreads.com/author/show/8164) |
| `full_name` | String | 100% | The author's full name |
| `birth_place` | String | 6% | The author's place of birth |
| `birth_year` | Integer | 4% | The author's year of birth |
| `death_year` | Integer | 2% | The author's year of death |
| `works` | List[int] | 99% | List of Work IDs authored by this person |

<br />

### Minhash signatures

**File**: [`minhash_signatures.jsonl`](https://zenodo.org/records/17609567/files/minhash_signatures.jsonl.gz?download=1) — `11,130,005` lines

**Example**

```json
{
  "source": "libgen",
  "id": "0af71d22806865ed567bc0fe2bc9be2a",
  "minhash_signature": [
    87053,
    62778,
    73327,
    ...
  ]
}
```

**Fields**

| Field | Type | Coverage | Description |
| --- | --- | --- | --- |
| `source` | String | 100% | Source dataset (`libgen` or `zlibrary`) |
| `id` | String / Integer | 100% | Unique ID in the source dataset (MD5 hash string for `libgen`, integer for `zlibrary`) |
| `minhash_signature` | List[int] | 100% | A list of 128 integers representing the document's MinHash signature (see Section 3.3 of the [paper](https://arxiv.org/abs/2511.11412)) |
