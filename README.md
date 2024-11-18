# term-is-empty

hugo v0.138.0+extended linux/amd64 BuildDate=unknown
https://discourse.gohugo.io/t/error-calling-data-term-is-empty-when-using-content-adapter-to-populate-tags/52457

data/tags/yugioh.json

```json
{
	"@context": [
		"https://www.w3.org/ns/activitystreams",
		{
			"@base": "https://trwnh.com/tags/"
		}
	],
	"id": "yu-gi-oh!",
	"name": "Yu-Gi-Oh!",
	"summary": "A media franchise including several series mostly revolving around the fictional card game of Duel Monsters.",
	"alsoKnownAs": ["yu-gi-oh", "ygo", "yugioh"]
}
```


content/tags/_content.gotmpl

```go
{{ $data := .Site.Data.tags }}
{{ $ctx := . }}


{{ range $data }}

	{{ $id := .id }}
	{{ $title := .name }}
	{{ $summary := .summary }}
	{{ $aliases := .alsoKnownAs }}

	{{ $page := dict
		"title" $title
		"summary" $summary
		"params" (dict "tag_aliases" $aliases "tag_name" $id)
		"path" .id
		"url" (printf "tags/%s" .id)
		"kind" "term"
	}}
	{{ $.AddPage $page }}

	{{ range $aliases }}
		{{ $page := dict
		"params" (dict "tag_canonical" $id "tag_name" .)
		"path" .
		"url" (printf "tags/%s" .)
		"kind" "term"
		}}
		{{ $.AddPage $page }}
	{{ end }}

{{ end }}
```

layouts/_default/taxonomy.html

```html
{{ define "body" }}
{{/*  this fails with "error calling Data: term is empty"  */}}
<dl>
{{ range .Data.Terms.ByCount }}
	<dt>{{.Name}} -- <a href="{{.Permalink}}">{{.Permalink}}</a></dt>
	<dd>used {{.Count}} time(s)</dd>
{{ end }}
</dl>
{{ end }}
```