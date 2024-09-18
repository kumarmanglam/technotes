
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
		bundleid: "",
		books: [
			{
				bookId: "",
				bookName: "",
				premium: true
			},
		]
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
		booksInLicenseId: "",
	}
]
```

## Collection: BooksInLicense

```json
[
	{
		booksInLicenseId: "",
		books: [
			{
				bookId: "",
				premium: true,
				concurrency: 1,
			},
			{
				bookId: "",
				premium: false,
				concurrency: "N/A",
			}
		]
	},
]
```


#### **Model Planning Version 2**:

1. **Bundle**:
    - The `Bundle` collection references `booksInBundleId`, which is a separate collection `BooksInBundle`. The actual books are decoupled from the bundle collection.
2. **BooksInBundle**:
    - Holds the list of books for each bundle separately. Each book has an associated `bundleId`, making it easier to manage books and their properties independently.
3. **License**:
    - Similar to `Bundle`, the `License` collection references `booksInBundleId`. The `BooksInLicense` collection manages the concurrency and premium status for each book independently, with an additional reference to `licenseId`.

**Pros**:

- **Normalization**: Data is normalized. Books are not directly embedded into bundles/licenses but referenced through `booksInBundleId` and `booksInLicense`.
- **Scalability**: Easier to manage updates for books (e.g., if the name or premium status changes) without affecting multiple bundles or licenses.
- **Separation of concerns**: Managing bundles and licenses separately allows more flexibility, especially for complex systems with a large number of books, licenses, and bundles.

**Cons**:

- **Complexity**: Slightly more complex to implement and query compared to Version 1 due to the additional collections (`BooksInBundle`, `BooksInLicense`).
- **Performance**: Depending on the database, fetching data might require more joins/lookups.