---
header-id: enabling-tags-categories-and-related-assets-for-guestbook-entries
---

# Enabling Tags, Categories, and Related Assets for Guestbook Entries

[TOC levels=1-4]

<div class="learn-path-step row">
    <p id="stepTitle">Adding Asset Features to Your UI</p><p>Step 4 of 5</p>
</div>

Enabling tags, categories, and related assets for guestbook entries is similar 
to enabling them for guestbooks. Please refer back to the previous step for
a detailed explanation. 

Open your `guestbook-web` module's `guestbookwebportlet/edit_entry.jsp` file. 
Replace its content with the following code: 

    <%@ include file="../init.jsp" %>

        <%
        long entryId = ParamUtil.getLong(renderRequest, "entryId");

        Entry entry = null;

        if (entryId > 0) {
            entry = EntryLocalServiceUtil.getEntry(entryId);
        }

        long guestbookId = ParamUtil.getLong(renderRequest, "guestbookId");
        %>

        <portlet:renderURL var="viewURL">
            <portlet:param 
                name="mvcPath" 
                value="/guestbookwebportlet/view.jsp" 
            />
        </portlet:renderURL>

        <liferay-ui:header
            backURL="<%= viewURL.toString() %>"
            title="<%= entry == null ? "Add Entry" : entry.getName() %>"
        />

        <portlet:actionURL name="addEntry" var="addEntryURL" />

        <aui:form action="<%= addEntryURL %>" name="fm">
            <aui:model-context bean="<%= entry %>" model="<%= Entry.class %>" />

                <aui:fieldset>
                    <aui:input name="name" />

                    <aui:input name="email" />

                    <aui:input name="message" />

                    <aui:input name="entryId" type="hidden" />

                    <aui:input name="guestbookId" type="hidden" 
                    value=
                    "<%= entry == null ? guestbookId : entry.getGuestbookId() %>" />
                </aui:fieldset>

        <liferay-ui:asset-categories-error />
                            <liferay-ui:asset-tags-error />
                            <liferay-ui:panel defaultState="closed" 
                            extended="<%= false %>" id="entryCategorizationPanel" 
                            persistState="<%= true %>" title="categorization">
                                    <aui:fieldset>
                                       <liferay-asset:asset-categories-selector className="<%= Entry.class.getName() %>" classPK="<%= entryId %>" />
                                       <liferay-asset:asset-tags-selector className="<%= Entry.class.getName() %>" classPK="<%= entryId %>" />
                                    </aui:fieldset>
                            </liferay-ui:panel>

                            <liferay-ui:panel defaultState="closed" 
                            extended="<%= false %>" id="entryAssetLinksPanel" 
                            persistState="<%= true %>" title="related-assets">
                                    <aui:fieldset collapsed="<%= true %>" collapsible="<%= true %>" label="related-assets">
					
    					<liferay-asset:input-asset-links
    						className="<%= Entry.class.getName() %>"
    						classPK="<%= entryId %>"
    					/>
					
    				</aui:fieldset>
                            </liferay-ui:panel>

            <aui:button-row>
                <aui:button type="submit" />

                <aui:button onClick="<%= viewURL.toString() %>" type="cancel" />
            </aui:button-row>
        </aui:form>

Test your JSP by using the Guestbook portlet to add and update Guestbook 
entries. Add and remove tags, categories, and related assets. 

| **Note:** Setting your custom asset as the *Main Asset* of a page is
| required to display related assets in the Related Assets portlet. This is done
| when creating
| [Friendly URLs](/docs/7-1/tutorials/-/knowledge_base/t/making-urls-friendlier)
| in a later step.

Well done! Next, you'll enable comments and ratings for guestbook entries. 
