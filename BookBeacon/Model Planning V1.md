

# Model Planning Version 1


## Collection: Bundle

```json
[
	{
		bundleId: "",
		bundleName: "",
		booksInBundle: [
			{
				bookId: "",
				bookName: "",
				premium: true
			}
		]
	}
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
	}
]
```



### Evaluation of the Model Versions

#### **Model Planning Version 1**:

1. **Bundle**:
    - The `Bundle` collection directly includes the books with `bookId`, `bookName`, and `premium` status in the array `booksInBundle`.
2. **License**:
    - Each `License` includes the `bundleId`, `bundleName`, and a list of books with their `concurrency` settings. Premium books have specific concurrency values, while normal books have `"N/A"`.

**Pros**:

- Simple and straightforward, no need for additional collections to manage relationships.
- Good for small data sets where bundles and licenses don’t change often.

**Cons**:

- **Data duplication**: If the same book exists in multiple bundles or licenses, you end up duplicating book data across multiple places.
- **Scalability**: This version could become inefficient as you add more books or need to manage larger bundles. Each time a book's details are updated, it needs to be updated in all bundles/licenses where it appears.