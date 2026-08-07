# config

A cell-based typed configuration sheet for Go, with YAML load/save.

A `Sheet` is a tree of typed `Cell`s. Each cell knows its name, type,
current value and default value. Applications register the schema up front
and then read or update individual cells by path segments.

## Features

- Typed cells (`bool`, `int`, `string`, `any`) with default values and
  runtime type validation/normalization.
- Disabled cells: ignored on YAML load/save, blocked from updates, always
  behave as their default value.
- Unknown keys from YAML are dropped on save (with a warning on load).
- Optional pluggable logger (`SetLogger`); discarded when unset.
- Depends only on [yamltree](https://github.com/he11ah0und/yamltree)
  (and transitively `gopkg.in/yaml.v3`).

## Usage

```go
sheet := config.NewSheet(config.SheetOptions{OnMissing: config.OnMissingError})

sheet.Register([]string{"log", "limit"}, config.TypeInt, 1000)
sheet.Register([]string{"feature", "beta"}, config.TypeBool, false, config.WithDisabled(true))

if err := sheet.LoadYAML(data); err != nil {
	log.Fatal(err)
}

limit := sheet.Int("log", "limit")
_ = sheet.Set([]string{"log", "limit"}, 500)

out, err := sheet.SaveYAML()
```
