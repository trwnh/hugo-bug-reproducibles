# page-resources-pipes-scss

- hugo v0.112.3+extended linux/amd64 BuildDate=unknown
- https://github.com/gohugoio/hugo/issues/11070

`layouts/_default/single.html`:
```
{{ define "head" }}
	{{ with .Resources.GetMatch "style.scss" }}
	{{ $style := . | toCSS | minify | fingerprint }} {{/* this line doesn't get rerun */}}
	<link rel="stylesheet" href="{{ $style.Permalink }}" integrity="{{ $style.Data.Integrity }}" />
	{{ end }}
{{ end }}

{{ define "body" }}
{{ .Content }}
{{ end }}
```

`content/section/page/style.scss`:
```
html {color: blue;} /* change this to red, it won't update */
```
