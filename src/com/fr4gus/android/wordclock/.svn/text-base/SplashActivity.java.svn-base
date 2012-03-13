package com.fr4gus.android.wordclock;

import android.app.Activity;
import android.content.Intent;
import android.os.Bundle;
import android.os.Handler;
import android.os.Message;

public class SplashActivity extends Activity implements Constants {
    public static final int CALL_NEXT = 1;

    Handler mHandle = new Handler() {

        @Override
        public void handleMessage(Message msg) {
            switch (msg.what) {
            case CALL_NEXT:
                Intent callNext = new Intent(CONTROL_ACTION);
                startActivity(callNext);
                finish();
                break;
            }
        }

    };

    /** Called when the activity is first created. */
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main);
        Thread th = new Thread() {

            @Override
            public void run() {
                try {
                    Thread.sleep(2000);
                } catch (InterruptedException e) {
                }
                mHandle.sendMessage(mHandle.obtainMessage(CALL_NEXT));
            }

        };
        th.start();
    }
}