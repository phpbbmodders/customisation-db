# Events for extensions

Titania dispatches these events so other phpBB extensions can add to its pages without changing Titania. PHP events are dispatched with phpBB's event dispatcher, like phpBB's own `core.*` events; template events work like phpBB's template events.

## New Revision page

| Event | Type | Where | Variables |
| --- | --- | --- | --- |
| `phpbb.titania.revision_create_upload` | PHP | `controller/contribution/revision.php`, `create()`, after the upload form is handled | `contrib`, `uploader`, `import_errors` |
| `phpbb.titania.revision_add_display` | PHP | `controller/contribution/revision.php`, `add()`, before the page is shown | `contrib`, `error` |
| `titania_revision_upload_after` | Template | `contributions/contribution_revision.html`, below the file uploader | |

`phpbb.titania.revision_create_upload` lets an extension attach a revision file it fetched for the author, such as a release from a code host. It calls `$uploader->add_local_file($path, $name, $mimetype)`, and the file goes through the same checks as a browser upload: allowed file types, the size limit (for anyone but team members) and Titania's files folder. It replaces a file uploaded before, as a new upload does. Errors added to `import_errors` are shown to the author.

## Contribution manage page

| Event | Type | Where | Variables |
| --- | --- | --- | --- |
| `phpbb.titania.contribution_manage_display` | PHP | `controller/contribution/manage.php`, `manage()`, before the page is shown | `contrib`, `error` |
| `phpbb.titania.contribution_manage_validate` | PHP | `manage()`, when the form is submitted, after Titania's own checks | `contrib`, `error` |
| `phpbb.titania.contribution_manage_submit_after` | PHP | `manage()`, after the contribution's changes were saved | `contrib` |
| `titania_contribution_manage_options_after` | Template | `contributions/contribution_manage.html`, at the end of the Options section | |

An extension that adds a field to the manage page adds its errors to `error` in `contribution_manage_validate`, which stops the changes being saved, and saves its field in `contribution_manage_submit_after`.

## Header

`styles/all/template/event/overall_header_navigation_append.html` adds the Customisation Database and Manage links to the header's Quick links on every page. Each can be turned off and renamed in Titania's config settings.
