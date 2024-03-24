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

> [Docker Forum](https://forums.docker.com/t/docker-credential-desktop-exe-executable-file-not-found-in-path-using-wsl2/100225)
