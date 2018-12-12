
# Multiselect List

### Use selection in filters

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

### Placeing List 

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

We can remove ```ml-selection-results``` div if we do not want to see number of selections, and ```Select all entries```, ```Clear Selection``` buttons

And not forgot to link script file:
```
<script src="~/Scripts/multiselect-list.js"></script>
```

In partial page for list we have format like this:
```
<table class="table table-striped table-card">
    <tr>
        <th>
            <label class="checkbox checkbox-inline">
                <input type="checkbox" class="ml-select-page" />
                <i class="input-helper"></i>
            </label>
        </th>
        <th>other columns</th>
        ...
    </tr>
    @foreach (var item in Model)
    {
        <tr>
            <td>
                <label class="checkbox checkbox-inline">
                    <input type="checkbox" class="ml-select" data-id="@item.UserID" />
                    <i class="input-helper"></i>
                </label>
            </td>
            <td>other columns</td>
            ...
        </tr>
    }
</table>
```

On footer paging part we have ```this``` as parameter for ```PagedOnComplete``` function:
```
OnComplete = "PagedOnComplete(this)"
```

### Calling js functions

```initMultiselectList()``` function called on document.ready, but if any case needed, you have to call again.

On ```PagedOnCompleteCustom``` you have call ```mlInitSelection``` Like this:

```
function PagedOnCompleteCustom(obj) {
    var listId = $(obj).attr('data-ajax-update').replace("#", "");
    mlInitSelection(listId);
}
```

For saveing or doing any action with ajax post use ```mlSelection``` parameter like this:

```
$.ajax({
    url: addinvitationVars.saveinvitation,
    type: 'POST',
    data: {
        EventID: parseInt($("#EventID").val()),
        isRequired: $("#isRequired")[0].checked,
        IsSendEmail: false,
        mlSelection: multiLists['divList']
    },
    success: function (data) {
        if (data.success)
            location.reload();
        else {
            $('#dialogContent').html(data.message);
            location.reload();
        }
    }
});
```

On action we have parameter like this:
```
public async Task<ActionResult> SaveInvitation(int EventID, bool isRequired, bool IsSendEmail, MultiselectListModel mlSelection)
{
    ...
}
```

