---
header-id: generating-results-summaries-0
---

# Generating Results Summaries

[TOC levels=1-4]

<div class="learn-path-step row">
    <p id="stepTitle">Enabling Search and Indexing for Entries</p><p>Step 3 of 5</p>
</div>

The Search application and the Asset Publisher application display results
retrieved from the search engine. You can control the display by implementing
a `ModelSummaryContributor`.

Create a `EntryModelSummaryContributor`:

    @Component(
            immediate = true,
            property = "indexer.class.name=com.liferay.docs.guestbook.model.Entry",
            service = ModelSummaryContributor.class
    )
    public class EntryModelSummaryContributor implements ModelSummaryContributor {

        @Override
        public Summary getSummary(
            Document document, Locale locale, String snippet) {

            Summary summary = createSummary(document);

            summary.setMaxContentLength(128);

            return summary;
        }

        private Summary createSummary(Document document) {
            String prefix = Field.SNIPPET + StringPool.UNDERLINE;

            String title = document.get(prefix + Field.TITLE, Field.CONTENT);
            String content = document.get(prefix + Field.CONTENT, Field.CONTENT);

            return new Summary(title, content);
        }

    }

First override `getSummary`, and set the maximum summary length on the summary
returned. The value `200` is a Liferay standard. Control the summary creation
in a utility method called `createSummary`. Guestbooks only included the title
in the summary, but Entries use the title and the content (the Entry message
field) to populate the summary.

Create summaries by combining key parts of the entity's data.

Now that the search and indexing logic is in place, you can update the service
layer so `add`, `update`, and `delete` service calls trigger the new logic.
