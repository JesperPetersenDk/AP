[**HOME**](../index.md)

## Android lifecycle

* Lifecycle methods are called in order starting from onCreate()

<img src="https://codelabs.developers.google.com/codelabs/android-training-activity-lifecycle-and-state/img/59d40f71d715436.png"/>


## Activities, Layouts, Services, and Views

* An activity represents a single screen with a user interface. Almost all activities interact with the user, so the Activity class takes care of creating a window in which a UI is places with setContentView(View).

* A layout defines the structure for a user interface in your app, such as in an activity.

* A service is a component which runs in the background without direct interaction with the user. As the service has no user interface, it is not bound to the lifecycle of an activity. Services are used for repetitive and potentially long running operations, i.e., Internet downloads, checking for new data, data processing, updating content providers and the like.

* In android, Layout is used to define the user interface for an app or activity and it will hold the UI elements that will appear to the user. The View is a base class for all UI components in Android.


## Android and Coroutines

### Example for IoT Legotrain

* A class is defined to take an IP address and a port to send a UDP packet to the train

      class UDPSender(private val host: String, private val port: Int) {
          fun send(packet: String) {
              try {
                  val message = packet.toByteArray()
                  val address = InetAddress.getByName(host)
                  val p = DatagramPacket(message, message.size, address, port)
                  val dsocket = DatagramSocket()
                  dsocket.send(p)
                  dsocket.close()
              } catch (e: Exception) {
                  System.err.println(e)
              }
          }
      }

* In onCreate() lifecycle method, a button is hooked up to a onClickListener
* When the button is clicked, the IP, port, speed and number of magnets to move are collected from four textfields
* For security reasons, Android SDK does not allow access to internet from the main thread. Therefore, a coroutine is created to handle the UDP packet

      class MainActivity : AppCompatActivity() {

          override fun onCreate(savedInstanceState: Bundle?) {
              super.onCreate(savedInstanceState)
              setContentView(R.layout.activity_main)

              send.setOnClickListener {
                  val address = host.text.toString()
                  val port = Integer.parseInt(port.text.toString())
                  val speed = speed.text.toString()
                  val magnets = magnets.text.toString()

                  GlobalScope.launch {
                      UDPSender(address, port).send("m,$magnets,$speed,700,880")
                  }
              }
          }
      }


## API’s for Android

The above example use the Anko API. The API is used for getting the send button with the id 'send'.

Kotlin way:
    val senderButton = findViewById(R.id.send) as Button
    
Anko way:
    val senderButton = send
