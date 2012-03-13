package com.fr4gus.android.wordclock;

import android.app.Activity;
import android.os.Bundle;
import android.util.Log;
import android.widget.CompoundButton;
import android.widget.SeekBar;
import android.widget.TimePicker;
import android.widget.ToggleButton;
import android.widget.CompoundButton.OnCheckedChangeListener;
import at.abraxas.amarino.Amarino;

/**
 * Activity that communicates with Arduino in order to read current state, then
 * allowing the user change its status (date, light intensity, dimming)
 * @author fr4gus
 *
 */
public class ControllerActivity extends Activity {
    TimePicker mPicker;

    SeekBar mBrightControl;

    ToggleButton mOrganicBreathingButton;

    final int DELAY = 150;

    long lastChange;

    /**
     * Device address
     */
    public static final String DEVICE_ADDRESS = "00:11:06:24:04:30";

    protected static final String TAG = "WordClockController";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.control);

        Amarino.connect(this, DEVICE_ADDRESS);

        mPicker = (TimePicker) findViewById(R.id.timePicker);
        mPicker.setOnTimeChangedListener(new TimePicker.OnTimeChangedListener() {

            @Override
            public void onTimeChanged(TimePicker view, int hourOfDay, int minute) {
                updateTime(hourOfDay, minute);
            }
        });

        mBrightControl = (SeekBar) findViewById(R.id.brightControl);
        mBrightControl.setOnSeekBarChangeListener(new SeekBar.OnSeekBarChangeListener() {

            @Override
            public void onProgressChanged(SeekBar seekBar, int progress, boolean fromUser) {
                // do not send to many updates, Arduino can't handle so much
                if (System.currentTimeMillis() - lastChange > DELAY) {
                    updateBrightness(seekBar.getProgress());
                    lastChange = System.currentTimeMillis();
                }
            }

            @Override
            public void onStartTrackingTouch(SeekBar seekBar) {
                lastChange = System.currentTimeMillis();
            }

            @Override
            public void onStopTrackingTouch(SeekBar seekBar) {
                updateBrightness(seekBar.getProgress());
            }
        });

        mOrganicBreathingButton = (ToggleButton) findViewById(R.id.organicBreathingButton);
        mOrganicBreathingButton.setOnCheckedChangeListener(new OnCheckedChangeListener() {

            @Override
            public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
                updateOrganicBreathing(isChecked);
                Log.d(TAG, "Activating organic breathing: " + isChecked);
            }
        });

    }

    @Override
    protected void onPause() {
        // TODO Auto-generated method stub
        super.onPause();
    }

    @Override
    protected void onStop() {
        super.onStop();
        // stop Amarino's background service, we don't need it any more 
        Amarino.disconnect(this, DEVICE_ADDRESS);
    }

    @Override
    protected void onSaveInstanceState(Bundle outState) {
        // TODO Auto-generated method stub
        super.onSaveInstanceState(outState);
    }

    private void updateTime(int hour, int minutes) {
        int time[] = new int[] { hour, minutes };
        Amarino.sendDataToArduino(this, DEVICE_ADDRESS, 't', time);
    }

    private void updateBrightness(int value) {
        Amarino.sendDataToArduino(this, DEVICE_ADDRESS, 'b', value);
    }

    private void updateOrganicBreathing(boolean activate) {
        int data = 0;
        if (activate){
            data = 1;
        }
        Amarino.sendDataToArduino(this, DEVICE_ADDRESS, 'o', data);
    }
}
