[![Gem Version](https://badge.fury.io/rb/rubocop-md.svg)](http://badge.fury.io/rb/rubocop-md)
[![Travis Status](https://travis-ci.org/palkan/rubocop-md.svg?branch=master)](https://travis-ci.org/palkan/rubocop-md)

# Rubocop Markdown

Run Rubocop against your Markdown files to make sure that code examples follow style guidelines and have valid syntax.

## Features

- Analyzes code blocks within Markdown files
- Shows correct line numbers in output
- Preserves specified language (i.e., do not try to analyze "\`\`\`sh")
- **Supports autocorrect 📝**

This project was developed to keep [test-prof](https://github.com/palkan/test-prof) guides consistent with Ruby style guide.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'rubocop-md'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install rubocop-md

## Usage

### Command line

Just require `rubocop-md` in your command:

```sh
rubocop -r "rubocop-md" ./lib
```

Autocorrect works too:

```sh
rubocop -r "rubocop-md" -a ./lib
```

### Configuration

Code in the documentation does not make sense to be checked for some style guidelines (eg `Style/FrozenStringLiteralComment`).

We described such rules in the [default config](config/default.yml), but if you use the same settings in your project’s `.rubocop.yml` file, `RoboCop` will override them.

Fortunately, `RuboCop` supports directory-level configuration and we can do the next trick.

At first, add `rubocop-md` to your main `.rubocop.yml`:

```yml
# .rubocop.yml

require:
 - "rubocop-md"
```

*Notice: additional options*

```yml
# .rubocop.yml

Markdown:
  # Whether to run RuboCop against non-valid snippets
  WarnInvalid: true
```

Secondly, create empty `.rubocop.yml` in your docs directory.

```bash
├── docs
│   ├── .rubocop.yml
│   ├── doc1.md
│   ├── doc2.md
│   └── doc3.md
├── lib
├── .rubocop.yml # main
└── ...
```

Third, just run

```bash
$ rubocop
```

Also you can add special rules in the second `.rubocop.yml`

```ruby
# rubocop.yml in docs folder

Metrics/LineLength:
  Max: 100

Lint/Void:
  Exclude:
    - '*.md'
```

## How it works?

- Preprocess Markdown source into Ruby source preserving line numbers
- Let RuboCop do its job
- Restore Markdown from preprocessed Ruby if it has been autocorrected

## Limitations

- RuboCop cache is disabled for Markdown files (because cache knows nothing about preprocessing)
- Uses naive Regexp-based approach to extract code blocks from Markdown, support only backticks-style code blocks\*
- No language detection included; if you do not specify language for your code blocks, you'd better turn `WarnInvalid` off (see above)

\* It should be easy to integrate a _real_ parser (e.g. [Kramdown](https://kramdown.gettalong.org)) and handle all possible syntax. Feel free to open an issue or pull request!

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake test` to run the tests.

To install this gem onto your local machine, run `bundle exec rake install`.

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/palkan/rubocop-md.

## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).
