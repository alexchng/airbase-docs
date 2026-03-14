# Project

The `project` subcommand helps developers administer various aspects of their project.

## Link

**Usage**

```bash
airbase project link <team handle> <project handle>
```

**Effect**

* my-project
  * .airbase
    * airbase.link.json (output)

**Sample Output**

```
$ airbase project link
? Overwrite existing link? (current: NextJS Hello World Template) Yes
? Select project
  Application A
  Application B
  Application C
❯ NextJS Hello World Template
  Application E
(Move up and down to reveal more choices)
? Select project NextJS Hello World Template
Link saved
```

## Example

### Specify team and project handle

```bash
$ airbase project link airbase agency-test
Link saved
```

## List

Display a list of project handles accessible from their current account.

**Usage**

```bash
airbase project list
```

**Sample Output**

```
agency-test
simple-next-app
template-nextjs-helloworld
```
