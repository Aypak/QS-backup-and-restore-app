#### ***Warning! Do not use for production apps ( yet )!***
#### ***Still in development!***

### Qlik Sense Mashup for Backup and Restore apps

Using the Serialize app from Alex we can export the QS app ( without the data ) to json file. This will include all objects - sheets, dimension and measures, stories etc. Then this file can be put under version control or can be stored just as a backup.

The next step is to use the json file and to import it back when is needed. And this is what part of this mashup is supposed to do.
This mashup provide both export the app to json and import the json file.

#### Supported objects
* sheets ( with all containing objects )
* stories
* master objects
* dimensions
* measures
* snapshots
* bookmarks
* ***variables ( in the next release )***
* app properties - like name, thumbnail, description etc.
* load script
* fields

#### Backup
During the export the app is not changed in any way. The generated file will be automatically downloaded to your download folder.

#### Restore
The restore process will "read" the existing app objects and will compare them with the objects which are present in the backup file.

* existing objects - the objects which are present in the current app and in the json file will be updated with the properties from the json file
* missing objects - objects that are present in the current app and not present in the json file will be deleted from the app
* new objects - objects that are present in the json file and not present in the current app will be created in the app

Few exclusions:
Some objects are excluded from the overall process ( for now )

* embedded media - the actual media files will not be deleted ( if needed ) and they will stay in the content library. At the current moment I haven't found a method that can include these files in the backup process.
* data connectors - connectors will not be deleted or inserted. I'm facing a bug with the method in the Engine API that when updating a connector assign new ID instead using the ID already provided for the json file

After the restore process is finished the app will be saved to preserve the changes. It's highly recommended to reload the app after the restore process ( my plan is to add this as an option in the following releases )

#### Usage

* Navigate to the mashup web page. The mashup will automatically establish connection with the QS Engine
* Pick an app from the dropdown and press "Open" ( can take some time. depends on the app size)
* At this point "Backup" button is active (if you need to only backup an app or just backup before restore)
* Choose the json backup file
* After this you will see the statistics - how many objects will be deleted, inserted and updated. Deleted objects will always be more. This number include all the sheets objects (including the sheets itself) but the json file count only the sheets ( the objects in the sheets are sheets children and they are included in the sheet object itself )
* press the "Restore" button and wait for the process to finish. After the process is done table will be displayed with more detail about the objects that were processed.
* That's it! If you have the app already open just refresh the browser tabs where this app is open.
