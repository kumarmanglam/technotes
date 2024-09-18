
# Model Planning Version 2


## Collection: Bundle

```json
[
	{
		bundleId: "",
		bundleName: "",
	}
]
```


## Collection: BooksInBundle

```json
[
	{
		bookId: "",
		bookName: "",
		premium: true,
		bundleId: "",
	},
]
```



## Collection: License

```json
[
	{
		licenseId: "",
		licenseName: "",
		bundleId: "",
		bundleName: "",
	}
]
```

## Collection: BooksInLicense

```json
[
	{
		bookId: "",
		premium: true,
		concurrency: 1,
		licenseId: "",
	},
]
```


