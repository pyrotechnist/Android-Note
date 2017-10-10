

* getFilesDir() returns a File object to a directory that is private to your application only. 
             When you use openFileOutput(String, int), the data you write is directly stored in this directory 
             and is not accessible by any other application. It holds your application files.

* getDir() enables you to create any file or directory in the internal memory, 
         which is also accessible by other applications depending on the mode you create it. 
         openFileOutput(String, int) will not work with the output of this 
         so you will have to use other means of writing and reading files to deal with this directory or file. 
         This is more used for creating custom files other than files only used by your application.
         
         
openFileOutput is specifically used for file writing into internal storage and disallow writing to external storage. However, FileOutputStream allows you to write to both internal and external storage as well. From my experience, you can create a directory with ease using FileOutputStream in internal storage. You can also set a mode using FileOutputStream as the 2nd parameter in one of its constructor. Example of how you write to internal storage using FileOutputStream in append mode:
         
         
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