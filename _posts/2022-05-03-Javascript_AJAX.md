---
layout: post
title:  "Cascading DropDownList using Javscript and AJAX"
date:   2022-05-03 08:00:00 -0400
categories: jekyll update
---
<h1 style="text-align: center;">MVC Core Cascading DropDownList</h1>

Source used: [MVC Core Cascading DropDownList][MVC Core Cascading DropDownList]

`InventoriesController.cs`

```c#
public IActionResult Create()
{
    var brands = new SelectList(_context.Brand.ToList(), "ID", "BrandName");
    var categories = new SelectList(_context.Category.ToList(), "ID", "CategoryName");
    var subCategories = new SelectList(_context.SubCategory.ToList(), "Id", "SubCategoryName");

    var viewModel = new Inventory
    {
        ItemDesc = null,
        ListofBrands = brands,
        ListofCategories = categories,
        ListofSubCategories = subCategories,
        LocationID = 0,
        DateEntered = DateTime.Now,
        DateChanged = DateTime.Now,
        DatePurchased = DateTime.Now,
        Quantity = 0
    };

    return View(viewModel);
}

public IActionResult getSubCategories(int id)
{
    var subCategories = new List<SubCategory>();
    subCategories = getSubCategoriesFromDatabaseByCategoryID(id); //call repository
    return Json(subCategories);
}

public List<SubCategory> getSubCategoriesFromDatabaseByCategoryID(int id)
{
    return _context.SubCategory.Where(c => c.CategoryId == id).ToList();
}
```

`Inventories\create.cshtml`

```html
<div>
    <label asp-for="CategoryID"></label>
    <select asp-for="CategoryID" asp-items="Model.ListofCategories" class="form-control" id="category-target"><option disabled selected>--- SELECT ---</option></select>
    <span asp-validation-for="CategoryID" class="text-danger"></span>
</div>
<div>
    <label asp-for="SubCategoryID"></label>
    <select asp-for="SubCategoryID" asp-items="Model.ListofSubCategories" class="form-control" id="subCategory-target"><option disabled selected>--- SELECT ---</option></select>
    <span asp-validation-for="SubCategoryID" class="text-danger"></span>
</div>
```

`This code resides within the create.cshtml file, but in a designated section for Javascript. @section Scripts { }`

```javascript
<script>
    $(document).ready(function () {
      $("#category-target").on("change", function () {
        $list = $("#subCategory-target");
        $.ajax({
            url: "/Inventories/getSubCategories",
            type: "GET",
            data: { id: $("#category-target").val() }, //id of the category which is used to extract subcategory
            traditional: true,
            success: function (result) {
                $list.empty();
                $.each(result, function (i, item) {
                    $list.append('<option value="' + item["id"] + '"> ' + item["subCategoryName"] + ' </option>');
                    console.log(item)
                });
            },
            error: function () {
                alert("Something went wrong call the police");
            }
        });
      });
    });
</script>
```

[MVC Core Cascading DropDownList]: https://stackoverflow.com/questions/41022433/asp-net-mvc-core-cascading-dropdownlist
