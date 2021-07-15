---
date: "2021-06-21T00:00:00Z"
tags:
- programming
title: Ignoring extra words in MeiliSearch v0.20
---

MeiliSearch is a search API, which is super easy to setup and has some amazing features like being typo tolerant, handling synonyms, highlighting, multi language support and more. Setting up MeiliSearch and working with it as a developer has been a smooth experience. The documentation is also well written.

But there is one problem! MeiliSearch fails to return when there is an extra useless word. For example, [here](https://crates.meilisearch.com/) is a Crates browser demo of MeiliSearch which searches through Rust crates. The demo is built by the MeiliSearch team and they also have a [blog](https://blog.meilisearch.com/search-rust-crates-meili/) post on it.

So when you search something like "crypto" in the Crates browser, it returns results within 5 milliseconds.

![https://crates.meilisearch.com/?q=crypto](https://imgur.com/54M0s74.png)

But when I add an extra random word like my name to the search query, no results are returned. 

![https://crates.meilisearch.com/?q=crypto+raghav](https://imgur.com/mMWZB5K.png)

# Fixing it

At [Neera](http://neera.ai/), we faced same issue was faced while integrating Pocket search. This is what we did to fix it. If MeiliSearch doesn't return any result, we trim the query and resend the search query to MeiliSearch and we continue this until a response is found. The query is trimmed from both the sides. First query is trimmed word by word from Right hand side till a result is obtained. If that fails, the query is trimmed word by word from Left hand side till MeiliSearch returns a response.

The following is the code in JS

```javascript
const client = new MeiliSearch({"host": "http://localhost:7700"})
const query = "A QUERY FROM THE USER CONTAINING USELESS WORDS"
const INDEX = "INDEX_FOR_DOCUMENTS"

let search_response;
search_response = client.index(INDEX).search(query);

// if MeiliSearch returns no results, i.e search_response.hits is an empty array, words are trimmed
const search_words_for_right_trim = search.split(" ");
const search_words_for_left_trim = search.split(" ");

// Till there is atleast one result, keep trimming words from the query
while(search_response.hits.length === 0 && (search_words_for_left_trim.length > 1 || search_words_for_right_trim.length > 1)) {

    // initially words are trimmed from the right hand side
        if (search_words_for_right_trim.length > 1) {
          search_words_for_right_trim.pop()
          const new_query = search_words_for_right_trim.join(' ')
          search_response = await client.index(INDEX).search(new_query);
        }
    // if trimming from right has not fetched any results, words are trimmed from the left hand side
        else if(search_words_for_left_trim.length > 1) {
          search_words_for_left_trim.shift()
          const new_query = search_words_for_left_trim.join(' ')
          search_response = await client.index(INDEX).search(new_query);
        }
      }
```

Why not trim character by character? So, if you trim character by character, many queries get wasted. MeiliSearch tolerates if there are 3 or 4 extra characters but fails, with just a single word. Hence, trimming word by word was prefered in this case.
