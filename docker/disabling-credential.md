# Disabling Credential

Change "credsStore" to "credStore"

then Docker will run without credentials .



\~/.docker/config.json

```json
{
	...
	"credsStore": "desktop",
	...
}
```

to

```json
{
	...
	"credStore": "desktop",
	...
}
```

