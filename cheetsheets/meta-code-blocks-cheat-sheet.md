# Creating and highlighting code blocks

Share samples of code with fenced code blocks and enabling syntax highlighting.

## Code Blocks

\<code\>
YOUR CODE OR FORMATTING HERE
\</code\>

## Rendered code block

<code>
YOUR CODE OR FORMATTING HERE
</code>

> You can tell GitHub to ignore (or escape) Markdown formatting by using \ before the Markdown character.
{.is-info}

## Fenced code blocks

You can create fenced code blocks by placing triple backticks <code>```</code> before and after the code block. We recommend placing a blank line before and after code blocks to make the raw formatting easier to read.

<code>
```
function test() {
  console.log("notice the blank line before this function?");
}
```
</code>

## Rendered fenced code block

```
function test() {
  console.log("notice the blank line before this function?");
}
```

Tip: To preserve your formatting within a list, make sure to indent non-fenced code blocks by eight spaces.

## Syntax highlighting

You can add an optional language identifier to enable syntax highlighting in your fenced code block.

## For example, to syntax highlight Ruby code:

<code>
```ruby
require 'redcarpet'
markdown = Redcarpet.new("Hello World!")
puts markdown.to_html
```
</code>

## Rendered code block with Ruby syntax highlighting

```ruby
require 'redcarpet'
markdown = Redcarpet.new("Hello World!")
puts markdown.to_html
```

GitHub uses Linguist to perform language detection and to select third-party grammars for syntax highlighting. You can find out which keywords are valid in the languages YAML file.

<https://github.com/github/linguist/blob/master/lib/linguist/languages.yml>
