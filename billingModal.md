
# Billing Modal

By using Billing Modal you can:
* Choose payment method
* Add/Edit payment method
* Choose billing address for payment method
* Add/Edit billing address

### Some pages we have Billing Modal
* Account\Addresses
* Account\PaymentMethods
* Account\Products
* Account\Subscription

### Placement

```
@Html.Partial("_BillingModal")

<script>
    var entityModel = {};
    var postBillingEntityUrl = '...';
</script>
<script src="~/Scripts/ps/BillingModal.js"></script>
```

```postBillingEntityUrl``` is the URL used for posting data after selecting payment method from modal
```entityModel``` is the data we are going to post. You can have any properties you want, and before posting ```paymentMethodID``` property will be attached to the object. So you are going to have ```paymentMethodID``` parameter in you action (```postBillingEntityUrl```) or ```paymentMethodID``` property in model, if you have a model of some class in your action.

To show modal we call ```loadPaymentMethods();``` function.

### Example
Lets look at example on ```Account\Products``` page:

```
@Html.Partial("_BillingModal")

@section Scripts {
    <script>
        ...

        var entityModel = {};
        var postBillingEntityUrl = "@Url.Action("ManualRenew", "Account")";

    </script>
    <script src="~/Scripts/ps/BillingModal.js"></script>
    <script src="~/Scripts/ps/MyProducts.js"></script>
}
```

```postBillingEntityUrl``` corresponding action looks like this:
```
public async Task<ActionResult> ManualRenew(int id, int discountID, int quantity, bool renew, int paymentMethodID)
{
  ...
}
```
As you can see last parameter is ```paymentMethodID```.

In JS file of this page we call our modal in some function, first setting data for post:
```
function ManualRenew(id, renew) {
    var count = $("#RenewLicenseCount_" + id).val();
    if (!count.match(/^\d+$/)) {
        count = 1;
    }

    entityModel = { id: id, discountID: $("#renewDiscountID_" + id).val(), quantity: count, renew: renew };

    loadPaymentMethods();
}
```
