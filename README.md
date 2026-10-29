## AIM:
 
To create a two screens , first screen will take one number input from user. After click on Factorial button, second screen will open and it should display factorial of the same number using Explicit Intents.


## EQUIPMENTS REQUIRED:
Latest Version Android Studio

## ALGORITHM:
```
Step 1: Input In MainActivity, capture the user input from the EditText control.

Step 2: Validation Check if the input is non-empty and falls within a valid range (0 to 20). If invalid, display an error message using a Toast.

Step 3: Launch Second Screen via Explicit Intent Create an Explicit Intent (new Intent(MainActivity.this, SecondActivity.class)), attach the input number using putExtra(), and start SecondActivity using startActivity().

Step 4: Receive Data & Calculate Factorial In SecondActivity, retrieve the passed integer from getIntent().getIntExtra() and compute its factorial using a loop.

Step 5: Display Result & Navigate Back Set the calculated factorial text on the TextView to display the result, and call finish() on button click to return to the first screen.
```
## PROGRAM:
```
/*
Program to print the text “ExplicitIntent”.
Developed by: Gowshik S
Registeration Number : 212223220026
*/
```
### MainActivity.java
```
package com.example.exp2b;

import android.content.Intent;
import android.os.Bundle;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

public class MainActivity extends AppCompatActivity {

    public static final String EXTRA_NUMBER = "com.example.exp2b.EXTRA_NUMBER";

    private EditText etInputNumber;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);

        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main), (v, insets) -> {
            Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);
            return insets;
        });

        etInputNumber = findViewById(R.id.etInputNumber);
        Button btnCalculate = findViewById(R.id.btnCalculate);

        btnCalculate.setOnClickListener(v -> openFactorialScreen());
    }

    private void openFactorialScreen() {
        String inputStr = etInputNumber.getText().toString().trim();

        if (inputStr.isEmpty()) {
            Toast.makeText(this, R.string.err_empty, Toast.LENGTH_SHORT).show();
            return;
        }

        try {
            int number = Integer.parseInt(inputStr);

            if (number < 0 || number > 20) {
                Toast.makeText(this, R.string.err_range, Toast.LENGTH_SHORT).show();
                return;
            }

            // Explicit Intent: target activity class is specified explicitly
            Intent explicitIntent = new Intent(MainActivity.this, SecondActivity.class);
            explicitIntent.putExtra(EXTRA_NUMBER, number);
            startActivity(explicitIntent);

        } catch (NumberFormatException e) {
            Toast.makeText(this, R.string.err_range, Toast.LENGTH_SHORT).show();
        }
    }
}

```
### Second MainActivity.java
```
package com.example.exp2b;

import android.content.Intent;
import android.os.Bundle;
import android.widget.Button;
import android.widget.TextView;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

public class SecondActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_second);

        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.second), (v, insets) -> {
            Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);
            return insets;
        });

        TextView txtFactorialResult = findViewById(R.id.txtFactorialResult);
        Button btnBackToMain = findViewById(R.id.btnBackToMain);

        Intent intent = getIntent();
        int inputNum = intent.getIntExtra(MainActivity.EXTRA_NUMBER, 0);

        long result = computeFactorial(inputNum);

        String displayText = getString(R.string.result_text, inputNum, result);
        txtFactorialResult.setText(displayText);

        btnBackToMain.setOnClickListener(v -> finish());
    }

    private long computeFactorial(int n) {
        long fact = 1;
        for (int i = 2; i <= n; i++) {
            fact *= i;
        }
        return fact;
    }
}

```
### Activity Main.xml
```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="24dp"
    tools:context=".MainActivity">

    <TextView
        android:id="@+id/txtMainTitle"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="80dp"
        android:text="@string/title_main"
        android:textColor="?attr/colorPrimary"
        android:textSize="26sp"
        android:textStyle="bold"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <EditText
        android:id="@+id/etInputNumber"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginTop="40dp"
        android:autofillHints="none"
        android:hint="@string/number_hint"
        android:inputType="number"
        android:minHeight="48dp"
        android:textSize="18sp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/txtMainTitle" />

    <Button
        android:id="@+id/btnCalculate"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginTop="32dp"
        android:text="@string/btn_factorial"
        android:textSize="16sp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/etInputNumber" />

</androidx.constraintlayout.widget.ConstraintLayout>

```
### Activity Second.xml
```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/second"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="24dp"
    tools:context=".SecondActivity">

    <TextView
        android:id="@+id/txtResultTitle"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="80dp"
        android:text="@string/title_result"
        android:textColor="?attr/colorPrimary"
        android:textSize="26sp"
        android:textStyle="bold"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <TextView
        android:id="@+id/txtFactorialResult"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginTop="40dp"
        android:gravity="center"
        android:textSize="22sp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/txtResultTitle" />

    <Button
        android:id="@+id/btnBackToMain"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginTop="40dp"
        android:text="@string/btn_back"
        android:textSize="16sp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/txtFactorialResult" />

</androidx.constraintlayout.widget.ConstraintLayout>

```
## OUTPUT
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a9a33f40-bed3-464a-aa33-97c1eb8a7fc1" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/acb4ca50-e7db-44d9-b4d9-960ef8b6f11a" />

## RESULT
Thus a Simple Android Application create a Explicit Intents using Android Studio is developed and executed successfully.


