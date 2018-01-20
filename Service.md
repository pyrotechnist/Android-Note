## IntentService ##

The IntentService is used to perform a certain task in the background. Once done, the instance of IntentService terminates itself automatically. An example for its usage would be downloading certain resources from the internet.

The IntentService class offers the onHandleIntent() method which will be asynchronously called by the Android system.

Creates a work queue that passes one intent at a time to your onHandleIntent() implementation

Process tasks one by one.

> You can use Broadcast to send notification to Activity 

In activity: add receiver to receive broadcast
```
    private BroadcastReceiver receiver = new BroadcastReceiver() {

        @Override
        public void onReceive(Context context, Intent intent) {
            Bundle bundle = intent.getExtras();
            if (bundle != null) {
                String string = bundle.getString(IntentTestService.FILEPATH);
                int resultCode = bundle.getInt(IntentTestService.RESULT);
                if (resultCode == RESULT_OK) {
                    Toast.makeText(MainActivity.this,
                            "Download complete. Download URI: " + string,
                            Toast.LENGTH_LONG).show();
                    textView.setText("Download done");
                } else {
                    Toast.makeText(MainActivity.this, "Download failed",
                            Toast.LENGTH_LONG).show();
                    textView.setText("Download failed");
                }
            }
        }
    };
    
    
    
        @Override
    protected void onResume() {
        super.onResume();
        registerReceiver(receiver, new IntentFilter(
                IntentTestService.NOTIFICATION));
    }
    @Override
    protected void onPause() {
        super.onPause();
        unregisterReceiver(receiver);
    }
    
  ```
  In intentservice: send broadcast
  
  ```
   Intent intent = new Intent(NOTIFICATION);
        intent.putExtra(FILEPATH, outputPath);
        intent.putExtra(RESULT, result);
        sendBroadcast(intent);
  ```
  
  
 > Or use handler to post information to ui thread 

```
@Override
public void onCreate() {
    super.onCreate();
    mHandler = new Handler();
}

@Override
protected void onHandleIntent(Intent intent) {
    mHandler.post(new Runnable() {            
        @Override
        public void run() {
            Toast.makeText(MyIntentService.this, "Hello Toast!", Toast.LENGTH_LONG).show();                
        }
    });
}
```


## Bound Services ##
