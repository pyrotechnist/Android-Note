




A Handler object registers itself with the thread in which it is created. It provides a channel to send data to this thread, for example the main thread. The data which can be posted via the Handler class can be an instance of the Message or the Runnable class. A Handler is particular useful if you have want to post multiple times data to the main thread.

You can post messages to it via the **sendMessage(Message)** or via the **sendEmptyMessage()** method. 

Use the post() method to send a **Runnable** to it.

There is 3 ways to use looper

### 1. Handler :override the handleMessage() method to process messages, post message via **sendMessage(Message)** or via the **sendEmptyMessage()**


#### sendEmptyMessage()
```
// test
public class MainActivity extends Activity {

    private ProgressDialog progressDialog;
    private ImageView imageView;
    private String url = "http://www.9ori.com/store/media/images/8ab579a656.jpg";
    private Bitmap bitmap = null;

    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        imageView = (ImageView) findViewById(R.id.imageView);

        Button start = (Button) findViewById(R.id.button1);
        start.setOnClickListener(new OnClickListener() {
        	@Override
            public void onClick(View arg0) {
        		progressDialog = ProgressDialog.show(MainActivity.this, "", "Loading..");
        		new Thread() {
        			public void run() {
        				bitmap = downloadBitmap(url);
        				messageHandler.sendEmptyMessage(0);
        			}
        		}.start();
            }
        });
    }

    private Handler messageHandler = new Handler() {
        public void handleMessage(Message msg) {
        	super.handleMessage(msg);
        	imageView.setImageBitmap(bitmap);
                progressDialog.dismiss();
        }
    };

    private Bitmap downloadBitmap(String url) {
        // Initialize the default HTTP client object
        final DefaultHttpClient client = new DefaultHttpClient();

        //forming a HttpGet request
        final HttpGet getRequest = new HttpGet(url);
        try {
            HttpResponse response = client.execute(getRequest);
            //check 200 OK for success
            final int statusCode = response.getStatusLine().getStatusCode();
            if (statusCode != HttpStatus.SC_OK) {
                Log.w("ImageDownloader", "Error " + statusCode + " while retrieving bitmap from " + url);
                return null;
            }

            final HttpEntity entity = response.getEntity();
            if (entity != null) {
                InputStream inputStream = null;
                try {
                    // getting contents from the stream
                    inputStream = entity.getContent();
                    // decoding stream data back into image Bitmap
                    Bitmap bitmap = BitmapFactory.decodeStream(inputStream);
                    return bitmap;
                } finally {
                    if (inputStream != null) {
                        inputStream.close();
                    }
                    entity.consumeContent();
                }
            }
        } catch (Exception e) {
            getRequest.abort();
            Log.e(getString(R.string.app_name), "Error "+ e.toString());
        }
        return null;
    }
}

```

#### sendMessage(Message)
```

public void buttonClick(View view)
{
    	
    	Runnable runnable = new Runnable() {
	        public void run() {
            	Message msg = handler.obtainMessage();
    			Bundle bundle = new Bundle();
    			SimpleDateFormat dateformat = 
                             new SimpleDateFormat("HH:mm:ss MM/dd/yyyy", Locale.US);
    			String dateString = dateformat.format(new Date());
    			bundle.putString("myKey", dateString);
                 msg.setData(bundle);
                 handler.sendMessage(msg);
	        }
      };
      
Thread mythread = new Thread(runnable);
mythread.start();
}
//Next, update the handleMessage() method of the handler to extract the date and time string from the bundle object in the message and display it on the TextView object: Handler handler = new Handler() {
@Override
public void handleMessage(Message msg) {			  
		Bundle bundle = msg.getData();
		String string = bundle.getString("myKey");
		TextView myTextView = 
                     (TextView)findViewById(R.id.myTextView);
		myTextView.setText(string);
	      }
}; 

```

### 2. Handler post : Use the post() method to send a Runnable to it.

```
public class MainActivity extends Activity {

	private Handler handler;
	private ProgressBar progressBar;

	/** Called when the activity is first created. */

	@Override
	public void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.main);
		handler = new Handler();
		progressBar = (ProgressBar) findViewById(R.id.progressBar1);

	}

	public void startProgress(View view) {

		new Thread(new Task()).start();
	}

	class Task implements Runnable {
		@Override
		public void run() {
			for (int i = 0; i <= 20; i++) {
				final int value = i;
				try {
					Thread.sleep(1000);
				} catch (InterruptedException e) {
					e.printStackTrace();
				}
				handler.post(new Runnable() {
					@Override
					public void run() {
						progressBar.setProgress(value);
					}
				});
			}
		}
	}

}
```

### 3. View post: The View class allows you to post objects of type Runnable via the post() method.
```
public class ProgressTestActivity extends Activity {
    private ProgressBar progress;
    private TextView text;

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main);
        progress = (ProgressBar) findViewById(R.id.progressBar1);
        text = (TextView) findViewById(R.id.textView1);

    }

    public void startProgress(View view) {
        // do something long
        Runnable runnable = new Runnable() {
            @Override
            public void run() {
                for (int i = 0; i <= 10; i++) {
                    final int value = i;
                     doFakeWork();
                    progress.post(new Runnable() {
                        @Override
                        public void run() {
                            text.setText("Updating");
                            progress.setProgress(value);
                        }
                    });
                }
            }
        };
        new Thread(runnable).start();
    }

    // Simulating something timeconsuming
    private void doFakeWork() {
        SystemClock.sleep(5000);e.printStackTrace();
    }

}
```


http://blog.csdn.net/lmj623565791/article/details/38377229
https://developer.android.com/training/multiple-threads/communicate-ui.html#Handler
http://www.techotopia.com/index.php/A_Basic_Overview_of_Android_Threads_and_Thread_handlers


