
# Basic page layout

If we have filters form for list and we need to use selections when applying filters, we have to add this hidden elements inside form:
```
<input type="hidden" id="mlIsSelectAll" name="mlIsSelectAll" />
<input type="hidden" id="mlItems" name="mlItems" />
```
Like this : 

```
<form method="get" data-ajax="true" data-ajax-mode="replace" data-ajax-update="#divList" data-ajax-success="PagedOnComplete(this)">
    <input type="hidden" id="mlIsSelectAll" name="mlIsSelectAll" />
    <input type="hidden" id="mlItems" name="mlItems" />
    <!-- other filter elements here -->
</form>
```

Also take attention on ```data-ajax-success``` attribute where we have ```this``` as parameter for ```PagedOnComplete``` function.

In js code when applying a filter you have to set values for this hidden elements:

```
$('#mlIsSelectAll').val(multiLists['divList'].isSelectAll);
$('#mlItems').val(JSON.stringify(multiLists['divList'].selectedItems));
```

For example: 
```
$("#SetIDs").on("change", function (e) {

    $('#mlIsSelectAll').val(multiLists['divList'].isSelectAll);
    $('#mlItems').val(JSON.stringify(multiLists['divList'].selectedItems));

    var cInput = $(this);
    var cForm = cInput.parents("form:first");
    cForm.submit();
});
```

You also have to add parameters on coressponding action. For Example see last 2 params:
```
public async Task<ActionResult> AddInvitation(string searchInvitedUser, string currentFilter, int? page, int? pageSize, int? EventID, List<string> SetIDs, bool? IsShowall, bool mlIsSelectAll = false, string mlItems = null)
{
  ...
  if (!string.IsNullOrEmpty(mlItems))
  {
      ModifiedCheckBoxsList = JsonConvert.DeserializeObject<List<int>>(mlItems);
  }
  ...
}
```

We change list placement to this format:

```
<div class="ml-list-container">
    <div class="ml-selection-results">
        <span>Selected <span class="ml-selected-count">0</span> of @Model.TotalItemCount entries.</span>
        <a href="#" class="ml-select-all">Select all entries</a>
        <a href="#" class="ml-clear-selection">Clear selection</a>
    </div>
    <div class="multiselect-list" id="divList">
        @Html.Partial("_AddInvitationsList")
    </div>
</div>
```

And not forgot to link script file:
```
<script src="~/Scripts/multiselect-list.js"></script>
```
