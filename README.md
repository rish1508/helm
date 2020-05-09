# helm

Helm charts deployments

## Chart Structure

* Chart.yml file with the metadata
* templates/ files are the k8s resources to be deployed
* values.yaml - Containing all the values
* charts/ , Chart.yml , requirements.yaml for the dependencies
* Documentation - LICENSE, README.md, values.schema.json, templates/NOTES.txt
* /templates/tests/ - for testing pod definition
* crds/ folder to create or add crds


## Helm Commands

* helm install [release] [chart]
* helm upgrade [release] [chart]
* helm rollback [release] [revision]
* helm history [release]
* helm status [release]
* helm get all [release] - compute the values 
* helm uninstall [release]
* helm  uninstall --keep-history [release]
* helm list
* helm get manifest [release]

### Debug

* helm template [chart]
* helm install [release] [chart] --debug --dry-run

### Passing values file

* values.yaml
* Other file  helm install -f file
* variables helm install --set foo=bar

### values.schema.json

add a json schema to validate the values file when ever helm install, template, upgrade command is called

Example : you can add some required values which are mandatory to pass 

### values.yaml

* parent values file can override the values of the child values 
* variables in the values file can have ( _ ) not ( - )
* Example metadata name could be {.Release.Name}-{.Chart.Name}-xxx
* you can use indent function to align values from values file in the yaml file
* you can use printf - %s-  %s to generate values with (-)
* use with and end to specify a scope and then you dont need to mention the full path

Example : 
(-) is used to remove un necessary spaces
{{ with .Values.service- }}
    {{ .port }}
{{-end }}

* if you have list of values and you want to add them in the yaml tamplete use example: {{ - range .Values.ingress.hosts }}
* if you want to optionally create a resource then you can have it with the if condition

{{- if Values.ingress.enabled }}
----
----
----

{{- end }}

* Use $variable if you want to use the variable globally
example : {{ $defaultPortName := .Values.port.name }}
example: {{ $.Release.Name }}

### _helper.tpl

* You can define your functions in the helper file 
* _helper file is in the template folder
* Example: to use function from helper {{ include "mychart.fullname" .}}

{{- define "mychart.fullname" -}}
{{- if Values.ingress.enabled }}
----
{{- else -}}
----
{{- end }}
{{- end -}}

.helmignore - in the template/ folder if you want to ignore some file

* To construct a string use the - list and join pipeline 

### global property

If you have defined a global property in the parent chart's values file it is available to all the child's values as well.

Example : {.Values.global.id}

### Library 

* use type:library in the Chart.yml if the chart is just a subtemplate and is not used to 
* only contains function to be shared and reuse by other charts

### NOTES.txt
* Used to output on the console
* content can be dynamically  added as its a template

### Helm Packaging

* helm package chart_name
* creates .tgz package

* helm repo index . (create index.htlm for the helm repo)

* helm package --sign key  (to sign your package)
* helm verify chart.tgz (to verify locally)
* helm install --verify (verifying when installing)

* helm repo list
* helm repo add myrepo http://repo_url
* helm repo remove myrepo

* helm dependency update guestbook
* helm dependency list guestbook
* helm dependency build guestbook (if you want to keep the same version everytime from chart.lock file because update command will get the new versions which may be yo do not need)




[https://github.com/phcollignon/helm3]