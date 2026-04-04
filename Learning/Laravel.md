
**Hal  yang perlu dipelajari**

1. Resource Controller
2. HTTP/Request
3. php artisan storage:link
4. CKEditor (Editor artucle)
5. laravel filemanager

**Upload Image**
validasi: 'required|image|file|mimes:png, jpg, jpeg, webp|max:2024'
input type file

pada controller
`$file = $request->file('img')`
`$fileName = uniqid().'.'.$file->getClientOriginalExtension();`
`$file->storeAs('public/', $fileName);`

`$data['img'] = $fileName`

`Article::create($data)`