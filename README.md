# Modular-Push-Buttons
Easy way to manage a lot of push buttons with a microcontroller

```cpp
class PushButton {
  public:
    String id;
    int pin;
    int db;
    int state;
    unsigned long lastChange = 0;

    PushButton(String id2 = "", int pin2 = -1, int db2 = -1) {
      this->id = id2;
      this->pin = pin2;
      this->db = db2;
      pinMode(pin, INPUT);
    };

    void update(unsigned long currentTime) {
      if (currentTime - this->lastChange < this->db) {return;}
      int newState = digitalRead(this->pin);
      if (newState == this->state) {return;}
      int oldState = this->state;
      this->state = newState;
      this->lastChange = currentTime;
      if (newState > oldState) {
        if (positiveTriggered != nullptr) {positiveTriggered(this);}
      } else {
        if (negativeTriggered != nullptr) {negativeTriggered(this);}
      }
    };

    // function to be run when button pressed
    void (*positiveTriggered)(PushButton* btn) = nullptr;
    void (*negativeTriggered)(PushButton* btn) = nullptr;

    void positiveTriggerConnect(void (*invoke)(PushButton* btn)) {
      positiveTriggered = invoke;
    }
    void positiveTriggerDisconnect() {
      positiveTriggered = nullptr;
    }

    void negativeTriggerConnect(void (*invoke)(PushButton* btn)) {
      negativeTriggered = invoke;
    }
    void negativeTriggerDisconnect() {
      negativeTriggered = nullptr;
    }
};
```
