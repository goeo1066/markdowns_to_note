# Disabling Credential

## Summary

Change "credsStore" to "credStore"

## Effect

then Docker will run without credentials .

## Example

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

> ref. [Docker Forum](https://forums.docker.com/t/docker-credential-desktop-exe-executable-file-not-found-in-path-using-wsl2/100225)
