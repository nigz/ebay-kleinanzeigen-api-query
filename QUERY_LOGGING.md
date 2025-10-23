# Query Parameter Logging Feature

## What was changed?

The `/inserate` endpoint now includes the search query parameter in each result item for easy tracking and logging.

## How it was done

Three simple changes were made to `scrapers/inserate.py`:

1. **Line 47**: Pass the query parameter to the `get_ads()` function
   ```python
   page_results = await get_ads(page, query)
   ```

2. **Line 64**: Update `get_ads()` function signature to accept query parameter
   ```python
   async def get_ads(page, query: str = None):
   ```

3. **Line 85**: Add the query field to each result dictionary
   ```python
   results.append({
       "adid": data_adid, 
       "url": data_href, 
       "title": title_text, 
       "price": price_text, 
       "description": description_text, 
       "query": query  # <- Added this field
   })
   ```

## Usage

Now when you call the API with a query parameter:

```
GET http://kleinanzeigen-api:8000/inserate?query=maximera
```

Each result will include the query that was used:

```json
{
  "success": true,
  "data": [
    {
      "adid": "3225335032",
      "url": "https://www.kleinanzeigen.de/s-anzeige/...",
      "title": "4 Winterreifen 195/65 R15...",
      "price": "199",
      "description": "Verkaufe 4 Winterreifen...",
      "query": "maximera"  ← The search query is now logged here
    }
  ]
}
```

You can easily access it via `data[0].query` to see which search parameter was used for each result.

## Benefits

- Simple and minimal implementation (only 3 lines changed)
- Easy to access: `data[0].query`
- Helpful for logging and tracking which search terms were used
- Backward compatible: query field will be `null` if no query parameter is provided
