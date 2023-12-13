mongodb-anonymizer
==================

## MongoDB anonymizer tool
Export your MongoDB database anonymized from source to target. Replace all sensitive data.
Thanks to `faker`.

[![oclif](https://img.shields.io/badge/cli-oclif-brightgreen.svg)](https://oclif.io)
[![License](https://img.shields.io/npm/l/mongodb-anonymizer.svg)](https://github.com/rap2hpoutre/mongodb-anonymizer/blob/main/package.json)

IMPORTANT: This tool does not copy/restore collection indexes.

## Usage

Install project dependencies:

```bash
yarn install
```

Run this command by giving a source URI and target URI:

> This command is going to fetch and copy all collections and related data from source database and insert them into the target database. If you don't provide any list of keys to be anonymized, the tool is going to use the default keys for anonymization: "email,name,description,address,city,country,phone,comment,birthdate"

```bash
npx mongodb-anonymizer \
  --uri=mongodb://localhost:27017/source \
  --targetUri=mongodb://localhost:27017/anonymized
```

☝️ Be careful, since target collections will be removed.

### Specify list of fields to anonymize

Use `--list` option with a comma separated list of key name:

> This command is going to anonymize the keys provided in the list argument for all collections which have these keys.

```bash
npx mongodb-anonymizer  \
  --uri=mongodb://localhost:27017/source \
  --targetUri=mongodb://localhost:27017/anonymized
  --list=email,firstName,lastName,phone
```

You can also specify replacements for a specific collection:

```csv
users.email,products.price
```

#### Customize replacements 

You can also choose which faker function you want to use to replace data (default is `faker.random.word`):

```bash
npx mongodb-anonymizer  \
  --uri=mongodb://localhost:27017/source \
  --targetUri=mongodb://localhost:27017/anonymized
  --list=firstName:faker.name.firstName,lastName:faker.name.lastName
```

:point_right: You don't need to specify faker function since the command will try to find correct function via key name.

You can use plain text too for static replacements:
```bash
npx mongodb-anonymizer  \
  --uri=mongodb://localhost:27017/source \
  --targetUri=mongodb://localhost:27017/anonymized
  --list=textcol:hello,jsoncol:{},intcol:12
```