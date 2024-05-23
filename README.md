# SMS-App
that app can be able to read sms &amp; show incoming sms 
public class MainActivity extends AppCompatActivity {

    private TextView smsTextView;
    private BroadcastReceiver smsReceiver;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        smsTextView = findViewById(R.id.textvew);
        smsReceiver = new BroadcastReceiver() {
            @Override
            public void onReceive(Context context, Intent intent) {
                if ("sms_received".equals(intent.getAction())) {
                    String sender = intent.getStringExtra("sender");
                    String messageBody = intent.getStringExtra("messageBody");
                    displayMessage(sender, messageBody);
                }
            }
        };
        IntentFilter intentfilter = new IntentFilter("sms_received");
        registerReceiver(smsReceiver, intentfilter);
    }
    protected  void onDestroy(){
        super.onDestroy();
        unregisterReceiver(smsReceiver);
    }
    private void displayMessage(String sender, String messageBody) {
        String displayText = "From: " + sender + "\nMessage: " + messageBody;
        smsTextView.setText(displayText);
    }
}
// Creat a java class 
public class smsrcvr extends BroadcastReceiver {


    @Override
    public void onReceive(Context context, Intent intent) {
   Bundle bundle =intent.getExtras();
        Object[] pdus = (Object[]) bundle.get("pdus");
   for (Object pdu : pdus){
        SmsMessage message = SmsMessage.createFromPdu((byte[]) pdu);
       String messageBody = message.getMessageBody();
       String sender = message.getOriginatingAddress();
       // Pass the received SMS information to MainActivity
       Intent smsIntent = new Intent("sms_received");
       smsIntent.putExtra("sender", sender);
       smsIntent.putExtra("messageBody", messageBody);
       context.sendBroadcast(smsIntent);
       Log.d("messageDetail","mobNo:"+sender+",msg:" +messageBody);
   }
    }
   }

