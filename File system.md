

* getFilesDir() returns a File object to a directory that is private to your application only. 
             When you use openFileOutput(String, int), the data you write is directly stored in this directory 
             and is not accessible by any other application. It holds your application files.

* getDir() enables you to create any file or directory in the internal memory, 
         which is also accessible by other applications depending on the mode you create it. 
         openFileOutput(String, int) will not work with the output of this 
         so you will have to use other means of writing and reading files to deal with this directory or file. 
         This is more used for creating custom files other than files only used by your application.
         
         
openFileOutput is specifically used for file writing into internal storage and disallow writing to external storage. However, FileOutputStream allows you to write to both internal and external storage as well. From my experience, you can create a directory with ease using FileOutputStream in internal storage. You can also set a mode using FileOutputStream as the 2nd parameter in one of its constructor. Example of how you write to internal storage using FileOutputStream in append mode:
         

# Internal storage

## Save to internal

To create a new file in one of these directories, you can use the File() constructor, 
passing the File provided by one of the above methods that specifies your internal storage directory. For example:  
  
``` 
File file = new File(context.getFilesDir(), filename);
```

Alternatively, you can call openFileOutput() to get a FileOutputStream that writes to a file in your internal directory.
For example, here's how to write some text to a file:


```
String filename = "myfile";
String string = "Hello world!";
FileOutputStream outputStream;

try {

  // solution 1 : File()  getFilesDir()
  // path to /data/data/yourapp/files
  File file = new File(context.getFilesDir(), filename);
  outputStream = new FileOutputStream(file);
  
  // solution 2 : openFileOutput();
  // path to /data/data/yourapp/files
  outputStream = openFileOutput(filename, Context.MODE_PRIVATE);
  
  // solution 3 : getDir()
  // path to /data/data/yourapp/app_imageDir
  File directory = cw.getDir("imageDir", Context.MODE_PRIVATE);
  File file=new File(directory,filename);
  outputStream = new FileOutputStream(file);
  
  
  outputStream.write(string.getBytes());
  outputStream.close();
} catch (Exception e) {
  e.printStackTrace();
}
```

## Load from internal

* Call openFileInput() and pass it the name of the file to read. This returns a FileInputStream.
* Read bytes from the file with read().
* Then close the stream with close().

```
 public void loadFromInternal(View v){

        String FILENAME = "hello_file";

        StringBuilder contentBuilder = new StringBuilder();

        try(BufferedReader br = new BufferedReader(new InputStreamReader(openFileInput(FILENAME))))
        {
            String sCurrentLine;
            while ((sCurrentLine = br.readLine()) != null)
            {
                contentBuilder.append(sCurrentLine).append("\n");
            }

        } catch (IOException e)
        {
            e.printStackTrace();
        }

        mTextView.setText(contentBuilder.toString());

    }

```


# External storage

## Save to External 

If you want to save public files on the external storage, use the getExternalStoragePublicDirectory() method to get a File representing the appropriate directory on the external storage. The method takes an argument specifying the type of file you want to save so that they can be logically organized with other public files, such as DIRECTORY_MUSIC or DIRECTORY_PICTURES. For example:

```
public File getAlbumStorageDir(String albumName) {
    // Get the directory for the user's public pictures directory.
    // path to /storage/sdcard/Documents/files/yourpath
    File file = new File(Environment.getExternalStoragePublicDirectory(
            Environment.DIRECTORY_PICTURES), albumName);
    if (!file.mkdirs()) {
        Log.e(LOG_TAG, "Directory not created");
    }
    return file;
}
```


If you want to save files that are private to your app, you can acquire the appropriate directory by calling getExternalFilesDir() and passing it a name indicating the type of directory you'd like. Each directory created this way is added to a parent directory that encapsulates all your app's external storage files, which the system deletes when the user uninstalls your app.

For example, here's a method you can use to create a directory for an individual photo album:

```
public File getAlbumStorageDir(Context context, String albumName) {
    // Get the directory for the app's private pictures directory.
    // path to /storage/sdcard/Android/data/com.longyuan.filesystemtest/files
    //File storageDir = getExternalFilesDir(null);
    // path to /storage/sdcard/Android/data/com.longyuan.filesystemtest/files/Documents
    File file = new File(context.getExternalFilesDir(
            Environment.DIRECTORY_PICTURES), albumName);
    if (!file.mkdirs()) {
        Log.e(LOG_TAG, "Directory not created");
    }
    return file;
}
```


## Load from external

```
String FILENAME = "hello_file";

        String string = "hello world!";

        // public path
        // path to /storage/sdcard/Documents/files/yourpath
        File storageDir = new File(
                Environment.getExternalStoragePublicDirectory(Environment.DIRECTORY_DOCUMENTS)
                        + "/GalleryTest");

        if (!storageDir.exists()) {
            Toast.makeText(this, "Cannot finfd  folder" + storageDir.getAbsolutePath(), Toast.LENGTH_LONG).show();
            return;
        }

        File imageFile = new File(storageDir, FILENAME);

        StringBuilder contentBuilder = new StringBuilder();

        try(BufferedReader br = new BufferedReader(new InputStreamReader(new FileInputStream (imageFile.getAbsolutePath()))))
        {
            String sCurrentLine;
            while ((sCurrentLine = br.readLine()) != null)
            {
                contentBuilder.append(sCurrentLine).append("\n");
            }

        } catch (IOException e)
        {
            e.printStackTrace();
        }

        mTextView.setText(contentBuilder.toString());
```

