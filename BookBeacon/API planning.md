
Product Bundle - Set of books - two category - premium/normal
premium books can have concurrency.
normal books cannot not have concurrency.

License - On a bundle - for every book set concurrency - for normal book - concurrency - N/A

Create a license - Product Bundle - 

API documentation

## `productBundle`

#### GET : `productBundle/bundles`
- Retrieve the list of bundles

- Response 
```json
{
	bundles: [
		{
			"bundleID": "",
			"bundleName": "",
		}
	]
}
```


#### Get: `productBundle/books/:bundleId` 
- Get the list of books in a specific bundle by its ID

- Response
```json
{
	bundleId: "",
	bundleName: "",
	books: [
		{
			"bookId": "",
			"bookName": "",
			"premium": true
		}
	]
}
```

## License :
#### GET `license/licenses`
get list of licenses

```json
{
	licenses:[
		{
			"licenseID": "",
			"licenseName": "",
			"bundleId": "",
			"bundleName" "",
		}
	]
}
```

#### Get `license/licenseById/:id` 
get a license by id

```json
{
	license: {
		"licenseID": "",
		"licenseName": "",
		"bundleId": "",
		"bundleName" "",
	}
}
```

#### Get `license/allbooks/:licenseID`
get a list of books by licenseID

```json
{
	licenseID: "",
	books: [
		{
			bookId: "",
			bookName: "",
			concurrency: 0,
			premium: true
		},
	]
}
```

#### POST `license/`
create a license

Request Body
```json
{
	license: {
		licenseName: "",
		concurrency: "",
	}
}
```

Response
```json
{
	license: {
		licenseID: "",
		licenseName: "",
		concurrency: "",
	}
}
```

