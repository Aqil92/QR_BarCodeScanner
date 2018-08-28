
Please Put Star if you like it.

# QR_BarCodeScanner
Step 1.  Add it in your root build.gradle at the end of repositories:

allprojects {
		repositories {
			...
			maven { url 'https://jitpack.io' }
		}
	}
  
  ------------------------------------------------------------------------------------------------------------------------------------------
  
Step 2.  Add the dependency

	dependencies {
	        implementation 'com.github.Aqil92:QR_BarCodeScanner:-SNAPSHOT'
	}
  
  ------------------------------------------------------------------------------------------------------------------------------------------
  
  
  Steps 3. Modify Your activity_main.xml to bellow activity_main.xml\
  <?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <!-- Barcode Reader fragment -->
    <fragment
        android:id="@+id/barcode_fragment"
        android:name="com.sevenrocks.qrandbarcodescanner.CodeReader.BarcodeReader"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:auto_focus="true"
        app:use_flash="false" />

    <!-- Scanner overlay animation -->
    <com.sevenrocks.qrandbarcodescanner.CodeReader.ScannerOverlay
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:background="#44000000"
        app:line_color="#7323DC"
        app:line_speed="6"
        app:line_width="4"
        app:square_height="200"
        app:square_width="200" />

</RelativeLayout>

------------------------------------------------------------------------------------------------------------------------------------------ 
  
Steps 4. Modify your MainActicity by bellow my MainActivity.java
MainAcivity.java
  
  public class MainActivity extends AppCompatActivity implements BarcodeReader.BarcodeReaderListener  {

    private BarcodeReader barcodeReader;
    public String   TAG="BarcodeScanner";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        barcodeReader = (BarcodeReader) getSupportFragmentManager().findFragmentById(R.id.barcode_fragment);
    }
    @Override
    public void onScanned(final Barcode barcode) {
        Log.e(TAG, "onScanned: " + barcode.displayValue);
        barcodeReader.playBeep();

        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                Toast.makeText(getApplicationContext(), "Barcode: " + barcode.displayValue, Toast.LENGTH_SHORT).show();
            }
        });
    }

    @Override
    public void onScannedMultiple(List<Barcode> barcodes) {
        Log.e(TAG, "onScannedMultiple: " + barcodes.size());

        String codes = "";
        for (Barcode barcode : barcodes) {
            codes += barcode.displayValue + ", ";
        }

        final String finalCodes = codes;
        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                Toast.makeText(getApplicationContext(), "Barcodes: " + finalCodes, Toast.LENGTH_SHORT).show();
            }
        });
    }

    @Override
    public void onBitmapScanned(SparseArray<Barcode> sparseArray) {

    }

    @Override
    public void onScanError(String errorMessage) {

    }

    @Override
    public void onCameraPermissionDenied() {
        Toast.makeText(getApplicationContext(), "Camera permission denied!", Toast.LENGTH_LONG).show();
        finish();
    }
}
