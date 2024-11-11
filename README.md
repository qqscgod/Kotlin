<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <!-- TextView to display result -->
    <TextView
        android:id="@+id/resultTextView"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:text="0"
        android:textSize="40sp"
        android:gravity="end"
        android:padding="16dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        android:background="#ECECEC"
        android:textColor="#000000"
        android:layout_marginEnd="16dp"
        android:layout_marginStart="16dp" />

    <!-- Buttons for digits and operators -->
    <GridLayout
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_marginTop="50dp"
        app:layout_constraintTop_toBottomOf="@id/resultTextView"
        android:columnCount="4"
        android:padding="16dp"
        android:layout_marginBottom="32dp">

        <!-- Row 1 -->
        <Button
            android:id="@+id/button7"
            style="@style/ButtonStyle"
            android:text="7" />
        <Button
            android:id="@+id/button8"
            style="@style/ButtonStyle"
            android:text="8" />
        <Button
            android:id="@+id/button9"
            style="@style/ButtonStyle"
            android:text="9" />
        <Button
            android:id="@+id/buttonDiv"
            style="@style/ButtonStyle"
            android:text="/" />

        <!-- Row 2 -->
        <Button
            android:id="@+id/button4"
            style="@style/ButtonStyle"
            android:text="4" />
        <Button
            android:id="@+id/button5"
            style="@style/ButtonStyle"
            android:text="5" />
        <Button
            android:id="@+id/button6"
            style="@style/ButtonStyle"
            android:text="6" />
        <Button
            android:id="@+id/buttonMul"
            style="@style/ButtonStyle"
            android:text="*" />

        <!-- Row 3 -->
        <Button
            android:id="@+id/button1"
            style="@style/ButtonStyle"
            android:text="1" />
        <Button
            android:id="@+id/button2"
            style="@style/ButtonStyle"
            android:text="2" />
        <Button
            android:id="@+id/button3"
            style="@style/ButtonStyle"
            android:text="3" />
        <Button
            android:id="@+id/buttonSub"
            style="@style/ButtonStyle"
            android:text="-" />

        <!-- Row 4 -->
        <Button
            android:id="@+id/button0"
            style="@style/ButtonStyle"
            android:text="0" />
        <Button
            android:id="@+id/buttonClear"
            style="@style/ButtonStyle"
            android:text="C" />
        <Button
            android:id="@+id/buttonEquals"
            style="@style/ButtonStyle"
            android:text="=" />
        <Button
            android:id="@+id/buttonAdd"
            style="@style/ButtonStyle"
            android:text="+" />
    </GridLayout>

</androidx.constraintlayout.widget.ConstraintLayout>

<resources>
    <style name="ButtonStyle">
        <item name="android:layout_width">0dp</item>
        <item name="android:layout_height">wrap_content</item>
        <item name="android:layout_margin">8dp</item>
        <item name="android:layout_rowWeight">1</item>
        <item name="android:textSize">18sp</item>
        <item name="android:textColor">#000000</item>
        <item name="android:background">#DDDDDD</item>
        <item name="android:gravity">center</item>
    </style>
</resources>

package com.example.calculatorapp

import android.os.Bundle
import android.view.View
import android.widget.Button
import android.widget.TextView
import androidx.appcompat.app.AppCompatActivity

class MainActivity : AppCompatActivity() {

    private lateinit var resultTextView: TextView
    private var currentInput: String = ""
    private var operator: String? = null
    private var operand1: Double? = null

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        resultTextView = findViewById(R.id.resultTextView)

        val buttons = listOf(
            R.id.button0, R.id.button1, R.id.button2, R.id.button3, R.id.button4, R.id.button5, R.id.button6,
            R.id.button7, R.id.button8, R.id.button9, R.id.buttonAdd, R.id.buttonSub, R.id.buttonMul, R.id.buttonDiv,
            R.id.buttonClear, R.id.buttonEquals
        )

        // Set listeners for all buttons
        for (buttonId in buttons) {
            val button = findViewById<Button>(buttonId)
            button.setOnClickListener {
                onButtonClicked(it)
            }
        }
    }

    private fun onButtonClicked(view: View) {
        val button = view as Button
        val buttonText = button.text.toString()

        when (buttonText) {
            "C" -> clear()
            "=" -> calculateResult()
            "+" -> setOperator("+")
            "-" -> setOperator("-")
            "*" -> setOperator("*")
            "/" -> setOperator("/")
            else -> appendToInput(buttonText)
        }
    }

    private fun clear() {
        currentInput = ""
        operand1 = null
        operator = null
        resultTextView.text = "0"
    }

    private fun appendToInput(input: String) {
        currentInput += input
        resultTextView.text = currentInput
    }

    private fun setOperator(op: String) {
        if (currentInput.isNotEmpty()) {
            operand1 = currentInput.toDouble()
            operator = op
            currentInput = ""
        }
    }

    private fun calculateResult() {
        if (currentInput.isNotEmpty() && operand1 != null && operator != null) {
            val operand2 = currentInput.toDouble()
            val result = when (operator) {
                "+" -> operand1!! + operand2
                "-" -> operand1!! - operand2
                "*" -> operand1!! * operand2
                "/" -> if (operand2 != 0.0) operand1!! / operand2 else "Error"
                else -> 0.0
            }
            resultTextView.text = result.toString()
            currentInput = result.toString()
            operand1 = null
            operator = null
        }
    }
}

