Route::get('/films/{id}', function () {
return "Show film details";
});

Route::post('/films', function () {
return "Store the film";
});

Route::get('/films/{id}/edit', function () {
return "Edit a film";
});

Route::patch('/films/{id}', function () {
return "Update the edited film";
});

Route::delete('/films/{id}', function () {
return "Update the edited film";
});

You won't be able to test the `POST`,`PATCH` or `DELETE` routes. But you should test all the `GET` routes to make sure they work.

<!-- For delete we need to use a POST action -->
<form method='POST' action='destroy.php'>
@csrf
@method("DELETE")
<input type="hidden" name="id" value="<?php echo $film['id'];?>">
<button type='submit'>Delete</button>
</form>
