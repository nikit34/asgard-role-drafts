## Test Data and Fixtures

### TestFixtures Object

```kotlin
object TestFixtures {
    fun advertItemDto(id: String = "test-id-1", title: String = "Test Advert", formattedPrice: String = "€100.00") =
        AdvertItemDto(id = id, title = title, formattedPrice = formattedPrice)
    fun collectionResponse(items: List<AdvertItemDto> = listOf(advertItemDto()), next: String? = null) =
        CollectionResponse(count = items.size, next = next, results = items)
    val emptyResponse = collectionResponse(items = emptyList())
}
```

### JSON Fixture Directory

```
src/androidTest/resources/fixtures/
    feature/{items_success,items_empty,items_page1,items_page2,item_detail}.json
    common/{config_bz,config_tj,config_mn,error_422_validation}.json
```

Rules: mirror real API response, realistic data, include `ui_texts`, use `formatted_price` (never raw), one fixture per scenario, never inline JSON.

### Multi-Market Data Factory

```kotlin
object MarketTestData {
    data class MarketConfig(val locale: String, val currency: String, val configFixture: String)
    val BZ = MarketConfig("en-US", "EUR", "common/config_bz.json")
    val TJ = MarketConfig("ru-RU", "TJS", "common/config_tj.json")
    val MN = MarketConfig("mn-MN", "MNT", "common/config_mn.json")
    val JA = MarketConfig("en-JM", "JMD", "common/config_ja.json")
    val PN = MarketConfig("en-TT", "TTD", "common/config_pn.json")
    val SL = MarketConfig("en-US", "EUR", "common/config_sl.json")
    val defaultMarkets = listOf(BZ, TJ, MN)
}
```

Don't: hardcode JSON inline, string-concatenate JSON, share mutable fixtures, use random data.
