# Breakdown of Each Story Completed

We began the sprint on Monday the 22nd of January. We had the first day to get familiar with the project, then we began on the actual stories. We had until Friday the next week to complete them. Below are the details of each story I did. Keep in mind for color styling that I had to stay within the site colors prescribed for me by the project manager.

## Create Model and Scaffolding

Here I made the model and created a derived context for it. Using the code first method, I created the database from the model and the context. Then I scaffolded the views and controllers for the CRUD pages and ran through the program and checked if my changes were saved to make sure the database and CRUD functionality all worked.

Here is the model
```C#
using System;
using System.Collections.Generic;
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;
using System.Linq;
using System.Web;

namespace TheatreCMS3.Areas.Production.Models
{
    [Table("ProductionPhotos")]
    public class ProductionPhoto
    {
        [Key]
        public int ProPhotoId { get; set; }
        public string Title { get; set; }
        public string Description { get; set; }
    }
}
```

I added my area to the dbcontext class identity model using this one line
```C#
public System.Data.Entity.DbSet<Areas.Production.Models.ProductionPhoto> ProductionPhotos { get; set; }
```

After that, I tested the database out and debugged some issues with its location. I resolved those issues, then I created the controller with MVC Entity Framework so that it scaffolded all the CRUD pages, controller methods, and views for me. At this point, I tested all the functionality of the database again, and all the of CRUD pages to make sure they all worked.


## Make Site-Wide Navbar

I created this partial view, I also nested a partial view for the login page in it. Then I nested the navbar in the main layout.cshtml page and I put the css in the main.css file to accomplish site-wide content. I used html helpers to nest the navbar and create links to other site pages. I also added bootstrap classes and my own css to override the bootstrap where I wanted a change. I grouped the center buttons under one div to center them and the rendered cshtml page buttons in another to align them on the right side.

```C#
<nav class="navbar navbar-expand-lg navbar-dark sticky-top nav-pills nav-fill">
  <div class="container-fluid">
    <a href="~/Home/Index"><img src="~/Content/images/TheatreLogo.png" class="navbar-icon-red" /></a>
    <ul class="nav">
      <li class="nav-item">
        <a class="nav-link" href="~/Home/Index">TheaterCMS</a>
      </li>
      <li class="nav nav-item dropdown">
        <a class="nav nav-link dropdown-toggle" href="#" id="navbarDropdownMenuLink" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
          Production
        </a>
        <div class="dropdown-menu dropdownbckgrndclr" aria-labelledby="navbarDropdownMenuLink">
          @Html.ActionLink("Cast Members", "Index", "CastMembers", new { Area = "Production" }, new { @class = "dropdown-item dropdowntxt" })
          @Html.ActionLink("Production Photos", "Index", "ProductionPhotos", new { Area = "Production" }, new { @class = "dropdown-item dropdowntxt" })
          @Html.ActionLink("Productions", "Index", "Productions", new { Area = "Production" }, new { @class = "dropdown-item dropdowntxt" })
        </div>
      </li>
      <li class="nav-item">
        <a class="nav-link" href="#">Rental</a>
      </li>
      <li class="nav-item">
        <a class="nav-link" href="#">Blog</a>
      </li>
    </ul>
  @Html.Partial("_LoginPartial")
  </div>
</nav>
```
I used CCS selectors to target elements and either apply new styling, or to override bootstrap classes
```CSS
/* Styling and positioning for the Navbar */
.nav > li {
    margin-left: .7vmin;
    margin-right: .7vmin;
    background-color: var(--secondary-color);
    border-radius: .5vmin;
    justify-content: center;
    text-align: center;
}

.nav > li:hover {
    background-color: #DEB887;
}

.nav > li > a {
    color: #000000;
    font-weight: bold;
}

.navbar-icon-red {
    height: auto;
    width: 15vmin;
    min-width: 50px;
}

.navbar {
    background-color: var(--main-bg-color);
    max-height: 55px;
    font-size: 1.4vmin;
}
```

## Style Create and Edit Pages

With this, I added bootstrap classes and my own to format the create and edit pages with the same bootstrap and css selectors, which increases code reusability. In essence, I styled the existing scaffolded content to be more user-friendly. Here I figured out how to add classes to html helper methods and learned how to debug using method definitions fairly well.

Here is the cshtml for the create page:
```C#
@model TheatreCMS3.Areas.Production.Models.ProductionPhoto

@{
    ViewBag.Title = "Create";
}


@using (Html.BeginForm())
{
    @Html.AntiForgeryToken()

    <h4 class="prodphoto-form-title card col-12 col-sm-12 col-md-6">Create Production Photo</h4>
    <div class="form-horizontal prodphoto-form-styling col-12 col-sm-12 col-md-6">
        <hr />
        @Html.ValidationSummary(true, "", new { @class = "text-danger" })
        <div class="form-group">
            @Html.LabelFor(model => model.Title, htmlAttributes: new { @class = "control-label col-md-2" })
            <div class="col-md-12 input-test">
                @Html.EditorFor(model => model.Title, new { htmlAttributes = new { @class = "form-control prodphoto-input-field", 
                 @placeholder = "Name your photo" } })
                @Html.ValidationMessageFor(model => model.Title, "", new { @class = "text-danger" })
            </div>
        </div>

        <div class="form-group">
            @Html.LabelFor(model => model.Description, htmlAttributes: new { @class = "control-label col-md-2" })
            <div class="col-md-12">
                @Html.EditorFor(model => model.Description, new { htmlAttributes = new { @class = "form-control prodphoto-input-field", 
                 @placeholder = "Describe your entry"} })
                @Html.ValidationMessageFor(model => model.Description, "", new { @class = "text-danger" })
            </div>
        </div>

        <div class="row prodphoto-btn-together justify-content-center">
            <div class="col-md-offset-2 prodphoto-btn-2">
                <input type="submit" value="Create" class="btn btn-default" />
            </div>

            @Html.ActionLink("Back to List", "Index", null, new {  @class = "btn prodphoto-btn" })
            
        </div>
    </div>
}



@section Scripts {
    @Scripts.Render("~/bundles/jqueryval")
}
```
Here is the edit page cshtml. You'll notice the styling is the same. This was to be efficient by increasing code reusability.
```C#
@model TheatreCMS3.Areas.Production.Models.ProductionPhoto

@{
    ViewBag.Title = "Edit";
}


@using (Html.BeginForm())
{
    @Html.AntiForgeryToken()

    <h4 class="prodphoto-form-title card col-12 col-sm-12 col-md-6">Edit Production Photo</h4>
    <div class="form-horizontal prodphoto-form-styling col-12 col-sm-12 col-md-6">
        <h4>@*ProductionPhoto*@</h4>
        <hr />
        @Html.ValidationSummary(true, "", new { @class = "text-danger" })
        @Html.HiddenFor(model => model.ProPhotoId)

        <div class="form-group">
            @Html.LabelFor(model => model.Title, htmlAttributes: new { @class = "control-label col-md-2" })
            <div class="col-md-12">
                @Html.EditorFor(model => model.Title, new { htmlAttributes = new { @class = "form-control prodphoto-input-field", @placeholder = "Name your photo" } })
                @Html.ValidationMessageFor(model => model.Title, "", new { @class = "text-danger" })
            </div>
        </div>

        <div class="form-group">
            @Html.LabelFor(model => model.Description, htmlAttributes: new { @class = "control-label col-md-2" })
            <div class="col-md-12">
                @Html.EditorFor(model => model.Description, new { htmlAttributes = new { @class = "form-control prodphoto-input-field", @placeholder = "Name your photo" } })
                @Html.ValidationMessageFor(model => model.Description, "", new { @class = "text-danger" })
            </div>
        </div>

        <div class="row prodphoto-btn-together justify-content-center">
            <div class="col-md-offset-2 prodphoto-btn-2">
                <input type="submit" value="Save" class="btn btn-default" />
            </div>

            @Html.ActionLink("Back to List", "Index", null, new { @class = "btn prodphoto-btn" })
        </div>
    </div>
}
```
Here you can see the CSS I used for both pages.
```CSS
.prodphoto-form-styling {
    background-color: var(--light-color);
    border-radius: 5px;
    margin-left: auto;
    margin-right: auto;
}

.prodphoto-form-title {
    padding: 5px;
    background-color: var(--light-color);
    text-align: center;
    margin-left: auto;
    margin-right: auto;
    margin-top: 8vh;
    white-space: nowrap;
}

.prodphoto-input-field {
    margin: 0 0 3vh 0;
    background-color: var(--light-color);
    background-color: #F5F5F5;
    width: 100%;
}

.prodphoto-input-field:focus, .input-field-changes:active {
    background-color: var(--light-color);
    box-shadow: 0 0 0 0.2rem rgba(93, 123, 244, 0.4);
}

.prodphoto-btn {
    background-color: rgba(93, 123, 244, .7);
    color: var(--dark-color);
    border-radius: 5px;
    width: auto;
    min-width: 7vw;
    margin-left: .4vw;
}

.prodphoto-btn-2 {
    background-color: var(--light-blue);
    border-radius: 5px;
    width: auto;
    text-align: center;
    min-width: 7vw;
}

.prodphoto-btn-together {
    padding: 10px;
    margin-right: auto;
    margin-left: auto;
}

.prodphoto-btn:hover {
    opacity: .8;
}

.prodphoto-btn-2:hover {
    opacity: .8;
}
```


## Index Page: Sort Production Photos by Title

Here I used bootstrap cards for the styling of the photo and description containers. This page is intended to show all the photos uploaded for each play in production. I managed this by using a hashset that selected unique names from the database and then passing that into a loop, which iterates through each unique title and then calls a nested loop. This loop compared each name for a match. If a match was found, it was added to the new list. This continued for each unique title name and then each new list of photos with titles that match would be displayed on the index page, using the form formatting and function in the nested loop.

```C#
@model IEnumerable<TheatreCMS3.Areas.Production.Models.ProductionPhoto>

@{
    ViewBag.Title = "Index";
}

<h1 class="prodphoto-title-spacing">Upcoming Plays</h1>

@* Need to make a list from the model that represents all unique names of plays, then in each loop, I need to have an if statement that gathers all same named items into the same row, with the title above it *@

@{ HashSet<string> uniqueTitle = new HashSet<string>(Model.Select(x => x.Title)); }

<div class="prodphoto-center-elements">
    @Html.ActionLink("Create New", "Create", null, new { @class = "badge badge-pill prodphoto-index-btn" })
</div>

@foreach (var item2 in uniqueTitle)
{
    <h4 class="prodphoto-center-elements prodphoto-card-title">@item2</h4>
    <div class="row justify-content-center">

        @foreach (var item in Model)
        {
            if (item.Title == item2)
            {
                <div class="col-9 col-sm-6 col-md-4">
                    <div class="card prodphoto-card">
                        <a href="@Url.Action("Details", null, new { id = item.ProPhotoId })">
                            <img class="card-img-top" src="~/Content/images/macbeth.png" alt="Card image cap">
                        </a>
                        <div class="card-body">
                            <h5 class="card-title">@Html.DisplayFor(modelItem => item.Title)</h5>
                            <p class="card-text prodphoto-desc">@Html.DisplayFor(modelItem => item.Description)</p>
                            <div class="prodphoto-center-elements">
                                @Html.ActionLink("Edit", "Edit", new { id = item.ProPhotoId }, new { @class = "badge badge-pill prodphoto-badge" })
                                @Html.ActionLink("Details", "Details", new { id = item.ProPhotoId }, new { @class = "badge badge-pill prodphoto-badge" })
                                @Html.ActionLink("Delete", "Delete", new { id = item.ProPhotoId }, new { @class = "badge badge-pill prodphoto-badge" })
                            </div>
                        </div>
                    </div>
                </div>
            }
        }
    </div>
}
```
Styling index page cards and page alignment
```CSS
.prodphoto-card {
    border: none;
    max-height: 55vh;
    margin-bottom: 3.5vh;
}

.prodphoto-desc {
    overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-line-clamp: 2; /* number of lines to show */
    -webkit-box-orient: vertical;
    min-height: 5.1vh;
}

.prodphoto-index-btn {
    background-color: var(--main-bg-color);
    color: var(--dark-color);
    border-radius: 5px;
    width: auto;
    min-width: 2vw;
    margin-bottom: 4vh;
}

.prodphoto-index-btn:hover {
    opacity: .7;
    color: var(--dark-color);
}

.prodphoto-center-elements {
    text-align: center;
}

.prodphoto-title-spacing {
    margin-top: 5vh;
    color: var(--light-color);
    text-align: center;
}

.prodphoto-badge {
    background-color: rgba(93, 123, 244, .8);
    color: var(--dark-color);
}

.prodphoto-badge:hover {
    background-color: rgba(93, 123, 244, 1);
    color: var(--dark-color);
}

.prodphoto-card-title {
    margin-top: 3vh;
    margin-bottom: 2vh;
    color: var(--light-color);
    text-align: center;
}
```
