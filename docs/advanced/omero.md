(omero-extension)=
# OMERO

This page describes how to use the OMERO extension in QuPath to access and interact with images hosted on OMERO servers. The installation instructions can be found [here](https://github.com/qupath/qupath-extension-omero).

:::{caution}
Several versions of the OMERO extension exist:

- The [QuPath Web OMERO extension](https://github.com/qupath/qupath-extension-omero-web) is a deprecated version developed by the QuPath team that only allows rendered RGB images to be accessed.
- The [BIOP OMERO extension](https://github.com/BIOP/qupath-extension-biop-omero) is a version developed by the BioImaging And Optics Platform (BIOP) to address the limitations of the QuPath Web OMERO extension by providing access to raw pixels of all types of images and by including more features. 
- The [QuPath OMERO extension](https://github.com/qupath/qupath-extension-omero) is a an improved version of the QuPath Web OMERO extension developped by the QuPath team. It allows more flexibility by letting the user choose between different methods to retrieve pixel values, and offers the same features as the BIOP OMERO extension. This is the version presented in this page.
:::


(omero-browsing)=
## Browsing an OMERO server

You can browse OMERO servers via the {menuselection}`Extensions --> OMERO --> Browse server...` command.

The menu will list all the servers that were opened in QuPath, from which you can choose the one to browse.

:::{figure} images/omero_browse_new_servers.png
:class: shadow-image small-image

How to browse an OMERO server
:::

:::{note}
The list of servers is the same one used in the `Web clients` window (see [Managing OMERO clients](omero-clients)).
:::

You can browse an existing OMERO server, or a new one (with which no connection was previously established) by clicking on {guilabel}`New server...`.
The URL is the one you use when accessing the OMERO server with a web browser (e.g. <https://idr.openmicroscopy.org>) and there are two possibilities for the credentials:

- You can check the `Log in as a public user` checkbox. That way, you will be able to browse the server without giving a username/password combination by using a public account (provided the server allows it). However, some features of the extension won't be available, namely the TODOOOOOOOOOOOOOOOOO ICE pixel access section and the [Send data to an OMERO server](sending-data) section.
- If you don't check the `Log in as a public user` checkbox, you will have to give your OMERO credentials but you won't have any of the restrictions mentionned above.

:::{figure} images/omero_browse_new_server.png
:class: shadow-image small-image

How to connect to a new OMERO server
:::

The browser will display all the projects, datasets, images, screens, plates, and wells filtered by OMERO group and owner. The design is very similar to OMERO's webclient.

:::{figure} images/omero_browser.png
:class: shadow-image mid-image

The OMERO browser (<http://idr.openmicroscopy.org>)
:::

You can open an individual image by double-clicking on it. Alternatively, select the relevant projects/datasets/images and click on {guilabel}`Import <entity> to QuPath`.

:::{Tip}
You can display more OMERO information on the selected file such as tags, key-value pairs and comments with {menuselection}`right click --> More info...`.
:::

To query a file on the OMERO server, one can also access the {guilabel}`Advanced...` feature, which will search the dataset for the input query.
Again, the results can be opened by either double-clicking on them or by selecting them and clicking on {guilabel}`Import <entity> to QuPath`.

:::{figure} images/omero_advanced_search.png
:class: shadow-image mid-image

Advanced search
:::


(adding-images)=
## Adding OMERO images to your project

QuPath can import OMERO images to a project via their links. This can be done in three ways:

- {menuselection}`File --> Open URI...` (Shortcut {kbd}`Ctrl + Shift + O`) and enter the URL of your OMERO image.
- {menuselection}`File --> Project --> Add images` and click 'Input URL' or 'From clipboard'.
- Via the OMERO browser (see [Browsing an OMERO server](omero-browsing)).

:::{admonition} URL formats
QuPath only accepts the following OMERO URL image formats:

- `<omero-web-server>/webclient/?show=image-<imageId>`
- `<omero-web-server>/webclient/img_detail/<imageId>/`
- `<omero-web-server>/webgateway/img_detail/<imageId>/`
- `<omero-web-server>/iviewer/?images=<imageId>`

E.g. <https://idr.openmicroscopy.org/webclient/?show=image-1920093>
:::

:::{tip}
The extension also accepts links to datasets and projects, in which case all compatible images inside them will be fetched and added to the current project.
The supported URL formats for projects and datasets are the following:

- `<omero-web-server>/webclient/?show=project-<projectId>`
- `<omero-web-server>/webclient/?show=dataset-<datasetId>`
:::

If no active connection with the OMERO server exists, QuPath will prompt you to enter your credentials, as mentionned in the [Browsing an OMERO server](omero-browsing) section:

:::{figure} images/omero_handle_credentials.png
:class: shadow-image small-image

How to give credentials
:::


(sending-data)=
## Send data to an OMERO server

Once an OMERO image is open in QuPath, you have the possibility to send data from QuPath to the OMERO server.

:::{caution}
Before sending information to an OMERO server, you must make sure that the user you are connected with has sufficient rights to write data to the OMERO server. This won't probably be the case if you are connected with the public account.
:::

(sending-annotations)=
### Send annotations

The extension can send annotations to the OMERO server. To do so, select the annotations on your image that you want to send back, then click on {menuselection}`OMERO --> Send to OMERO --> Send Annotations`. The annotations will be represented as `ROIs` on the OMERO server.

::::{list-table}
:widths: 50 50
:align: center

* - :::{image} images/omero_send_annotations.png
    :class: shadow-image
    :width: 100%
    :::
  - :::{image} images/omero_send_annotations_form.png
    :class: shadow-image
    :width: 100%
    :::
* - Send annotations from QuPath to the OMERO server
  - Parameters selection
::::

You have the possibility to delete annotations already present on the OMERO server. Since some groups allow multiple users to send annotations to the same image, you can delete the annotations of all or of one specific user. Deleting annotations of different users might not always work due to permissions.

:::{Note}
The user interface only lets the possibility to send annotations, not detections. This is because OMERO does not well support sending thousands of objects. If you do so, you might make the entire server unresponsive.
:::

:::{caution}
If you open the OMERO image with the `OMERO.iviewer`, you will see the annotations. They will have a comment that you shouldn't modify; it is used by the extension to store QuPath specific information on the annotation (e.g. class name).

:::{figure} images/omero_annotation.png
:class: shadow-image small-image
An annotation on the OMERO server
:::

:::

If the QuPath image contains annotation or detection measurements (see {menuselection}`Measure --> Show annotation measurements` or {menuselection}`Measure --> Show detection measurements`), they can also be sent to the OMERO server. They will be stored as `Attachments` on the OMERO server.

:::{figure} images/omero_measurements_attachment.png
:class: shadow-image small-image

Annotation measurements on the OMERO server
:::

### Send key-value pairs

Each image of a QuPath project has metadata (which consist of key-value pairs) attached to it. This metadata can be sent to the OMERO server by clicking on {menuselection}`OMERO --> Send to OMERO --> Send key-value pairs`.

::::{list-table}
:widths: 50 50
:align: center

* - :::{image} images/omero_send_kvp.png
    :class: shadow-image
    :width: 100%
    :::
  - :::{image} images/omero_send_kvp_form.png
    :class: shadow-image
    :width: 100%
    :::
* - Send key-value pairs from QuPath to the OMERO server
  - Parameters selection
::::

You can select which pairs to send, as well as how to proceed if there is two keys are duplicate (replace or keep existing keys). An OMERO server can store multiple keys, while keys in QuPath are unique.

In OMERO, key-value pairs can be grouped by a `namespace`. You can choose which namespace to use when sending key-value pairs.

:::{figure} images/omero_kvp_namespace.png
:class: shadow-image small-image

Key-value pairs with the default and the `qupath` namespace on the OMERO server
:::

### Send image settings

In QuPath, the channel names, colors, and display ranges of the current image can be found by clicking on {menuselection}`View --> Brightness/Contrast`. This data (as well as the image name is a project is open) can be sent to OMERO by clicking on {menuselection}`OMERO --> Send to OMERO --> Send image settings`.

::::{list-table}
:widths: 50 50
:align: center

* - :::{image} images/omero_send_image_settings.png
    :class: shadow-image
    :width: 100%
    :::
  - :::{image} images/omero_send_image_settings_form.png
    :class: shadow-image
    :width: 100%
    :::
* - Send image settings from QuPath to the OMERO server
  - Parameters selection
::::


## Import data from an OMERO server

Once an OMERO image is open in QuPath, you have the possibility to import data from the OMERO server to QuPath.

### Import annotations

The extension can import ROIs stored on an OMERO server and convert them to QuPath annotations by clicking on {menuselection}`OMERO --> Import from OMERO --> Import annotations`.

::::{list-table}
:widths: 50 50
:align: center

* - :::{image} images/omero_import_annotations.png
    :class: shadow-image
    :width: 100%
    :::
  - :::{image} images/omero_import_annotations_form.png
    :class: shadow-image
    :width: 100%
    :::
* - Import annotations from an OMERO server to QuPath
  - Parameters selection
::::

It is possible to delete all existing annotations and detections of the current QuPath image before importing annotations.

:::{Note}
If the ROIs to import were created by this extension, the created annotations will have the same properties (e.g. class name) as the annotations that were used to create the ROIs (see the [Send annotations](sending-annotations) section).
:::

### Import key-value pairs

An image stored on an OMERO server can store key-value pairs. This data can be imported to QuPath and added to the metadata of the project entry by clicking on {menuselection}`OMERO --> Import from OMERO --> Import key-value pairs`.

::::{list-table}
:widths: 50 50
:align: center

* - :::{image} images/omero_import_kvp.png
    :class: shadow-image
    :width: 100%
    :::
  - :::{image} images/omero_import_kvp_form.png
    :class: shadow-image
    :width: 100%
    :::
* - Import key-value pairs from an OMERO server to QuPath
  - Parameters selection
::::

In OMERO, key-value pairs can be grouped by a `namespace`. You can select which namespaces to consider when importing key-value pairs.

QuPath metadata can only have unique keys. Therefore, you can specify what to do when a duplicate happens (keep or replace the existing value).

### Import image settings

The channel names, colors, and display ranges of the current image, as well as the image name, can be imported from OMERO by clicking on {menuselection}`OMERO --> Import from OMERO --> Import image settings`.

::::{list-table}
:widths: 50 50
:align: center

* - :::{image} images/omero_import_image_settings.png
    :class: shadow-image
    :width: 100%
    :::
  - :::{image} images/omero_import_image_settings_form.png
    :class: shadow-image
    :width: 100%
    :::
* - Import image settings from an OMERO server to QuPath
  - Parameters selection
::::

You can select what specific parameters you want to update.


(omero-clients)=
## Managing OMERO clients

QuPath allows you to manage all active and inactive OMERO clients. To display a pane with all OMERO clients, navigate to {menuselection}`Extensions --> OMERO --> Manage server connections`.

There, a window displaying the servers to which a previous connection was made, allows you to log in, browse, log out and remove (forget) it.

If an account with authentication is currently connected to the server, its username will be written in parenthesis next to the server’s URL.

The green circle next to a server indicates whether there is an active connection to the server. The green circle next to the images’ URI indicates whether the image can be accessed with the current account.

:::{figure} images/omero_manage_clients.png
:class: shadow-image small-image

Manage OMERO server connections
:::

In the example above, the user `user` is connected to the first server (<http://localhost:4080>) and the connection with the second server (<http://idr.openmicroscopy.org>) uses the public account (no username/password).

:::{Note}
- Different OMERO accounts can be logged into different remote servers simultaneously (e.g. account A to server X and account B to server Y). But a remote server can only be reached with **one** OMERO account simultaneously.

- Be aware that the `Manage server connections` window displays how QuPath handles connections to OMERO servers. It does not directly relate to your project. E.g. deleting a project entry or switching projects will not affect the OMERO connections in any way, and vice-versa.
:::
