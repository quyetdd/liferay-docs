---
header-id: querying-for-entry-documents
---

# Querying for Entry Documents

[TOC levels=1-4]

<div class="learn-path-step row">
    <p id="stepTitle">Enabling Search and Indexing for Entries</p><p>Step 3 of 5</p>
</div>

The code is in place for indexing Entries to the search engine. Next code the
behavior necessary for querying the indexed documents.

Implement two classes:

1.  `EntryKeywordQueryContributor` contributes clauses to the ongoing search
    query.

2.  `EntryModelPreFilterContributor` controls how search results are filtered
    before they're returned from the search engine.

## Implementing `KeywordQueryContributor`

Create `EntryKeywordQueryContributor` and populate it with this:

    @Component(
            immediate = true,
            property = "indexer.class.name=com.liferay.docs.guestbook.model.Entry",
            service = KeywordQueryContributor.class
    )
    public class EntryKeywordQueryContributor implements KeywordQueryContributor {

        @Override
        public void contribute(
            String keywords, BooleanQuery booleanQuery,
            KeywordQueryContributorHelper keywordQueryContributorHelper) {

            SearchContext searchContext =
        keywordQueryContributorHelper.getSearchContext();

            queryHelper.addSearchLocalizedTerm(
        booleanQuery, searchContext, Field.TITLE, false);
            queryHelper.addSearchLocalizedTerm(
        booleanQuery, searchContext, Field.CONTENT, false);
            queryHelper.addSearchLocalizedTerm(
        booleanQuery, searchContext, "entryEmail", false);
        }

        @Reference
        protected QueryHelper queryHelper;

    }

Adding the localized search terms is important. For all localized Entry fields
in the index, retrieve the localized value from the search engine.

Now that the query code is in place, you can define how returned Entry documents
are summarized. 
