package com.example.typingspeedtest;

import androidx.appcompat.app.AppCompatActivity;
import android.os.Bundle;
import android.os.Handler;
import android.text.Editable;
import android.text.TextWatcher;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;
import java.util.Random;

public class MainActivity extends AppCompatActivity {

    private TextView tvGeneratedText, tvTimer, tvResult;
    private EditText etUserInput;
    private Button btnStart;

    private String[] sampleTexts = {
            "The quick brown fox jumps over the lazy dog.",
            "Typing fast requires practice and focus.",
            "Android development is fun and rewarding.",
            "Consistency is key to improvement.",
            "Never stop learning and growing."
    };

    private Handler handler;
    private int timeLeft = 60; // 60 seconds for the test
    private boolean isTestRunning = false;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        tvGeneratedText = findViewById(R.id.tvGeneratedText);
        tvTimer = findViewById(R.id.tvTimer);
        tvResult = findViewById(R.id.tvResult);
        etUserInput = findViewById(R.id.etUserInput);
        btnStart = findViewById(R.id.btnStart);

        handler = new Handler();

        btnStart.setOnClickListener(view -> startTypingTest());

        etUserInput.addTextChangedListener(new TextWatcher() {
            @Override
            public void beforeTextChanged(CharSequence s, int start, int count, int after) {}

            @Override
            public void onTextChanged(CharSequence s, int start, int before, int count) {}

            @Override
            public void afterTextChanged(Editable s) {
                if (isTestRunning) {
                    checkTypingAccuracy();
                }
            }
        });
    }

    private void startTypingTest() {
        // Reset everything
        etUserInput.setText("");
        tvResult.setText("");
        timeLeft = 60;
        isTestRunning = true;

        // Generate random text
        String randomText = sampleTexts[new Random().nextInt(sampleTexts.length)];
        tvGeneratedText.setText(randomText);

        // Start countdown timer
        handler.postDelayed(updateTimerRunnable, 1000);
    }

    private final Runnable updateTimerRunnable = new Runnable() {
        @Override
        public void run() {
            if (timeLeft > 0) {
                timeLeft--;
                tvTimer.setText("Time Left: " + timeLeft + "s");
                handler.postDelayed(this, 1000);
            } else {
                isTestRunning = false;
                handler.removeCallbacks(this);
                calculateResults();
            }
        }
    };

    private void checkTypingAccuracy() {
        String generatedText = tvGeneratedText.getText().toString();
        String userInput = etUserInput.getText().toString();

        if (generatedText.startsWith(userInput)) {
            etUserInput.setTextColor(getResources().getColor(android.R.color.holo_green_dark));
        } else {
            etUserInput.setTextColor(getResources().getColor(android.R.color.holo_red_dark));
        }
    }

    private void calculateResults() {
        String generatedText = tvGeneratedText.getText().toString();
        String userInput = etUserInput.getText().toString();

        int correctChars = 0;
        for (int i = 0; i < Math.min(generatedText.length(), userInput.length()); i++) {
            if (generatedText.charAt(i) == userInput.charAt(i)) {
                correctChars++;
            }
        }

        int wordsTyped = userInput.split("\\s+").length;
        tvResult.setText("Words Typed: " + wordsTyped + "\nAccuracy: " + (correctChars * 100 / generatedText.length()) + "%");
    }
}
