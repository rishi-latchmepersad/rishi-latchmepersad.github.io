---
title: Unit Testing
---

I spent the last week or so researching testing in the embedded world. There's definitely a ton of depth in this field, and I know that there are folks spending their entire
workday writing tests. It will be a long time before I get to that level, but I wanted to share some of the things I learned.

I started off by looking into Pytest on the ESP32. ESP has a really nice guide here: [ESP-IDF PyTest Testing](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/contribute/esp-idf-tests-with-pytest.html),
which I followed to get some basic tests running. I used my basic hello world ESP project to set up the tests, since there were some examples in the Pytest embedded github
repo which referenced it: [Pytest Embedded Examples](https://github.com/espressif/pytest-embedded/tree/main/examples/esp-idf).

However, I found the documentation and the repo lacking. I ended up needing to add a bunch of files manually to my project (eg. conftest.py, pytest.ini, etc.) to get it to work.
In the future, I'll probably have to create my own Youtube tutorial to help others get started with Pytest on the ESP32.

For now though, I just wanted to understand how it worked. As far as I understand so far, Pytest looks for output strings in the ESP logs that match certain patterns.

For example, the below code block looks for the string "Hello", and the string "Restarting". 
```
def test_hello_world(dut):
    # expect from what esptool.py printed to sys.stdout
    dut.expect("boot: End of partition table")

    # now the `dut.expect` would return a `re.Match` object if succeeded
    res = dut.expect(r"Hello (\w+)!")

    # don't forget to decode, since the serial outputs are all bytes
    logging.info(f'hello to {res.group(1).decode("utf8")}')

    # of course you can just don't care about the return value, do an assert only :)
    dut.expect("Restarting")

```

Once these strings are found, the test passes. I realized a big problem here though. Since the code was mostly going to be written in C, we wouldn't be able to actually import
and test the functions like we would need to do in unit tests. I guess that's why people were saying that unit tests are done in another framework, and integration tests are done
with Pytest.

I then moved to read up on the Unity framework, which is a C unit testing framework. I found a great guide on the Espressif website: [Unity Unit Testing in ESP](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-guides/unit-tests.html).

I also pulled up the homepage for Unity: [Unity Test Framework](https://github.com/ThrowTheSwitch/Unity/tree/master)

This one made a lot more sense to me for unit testing in embedded, since we could actually import our C functions.

I got distracted a bit though, so I didn't end up writing these tests in my ESP32 microcontroller project. I ended up switching back to my STM32 project. However, Unity
seems to be the preferred framework for both platforms (and probably most other embedded platforms), so I figured it wouldn't matter.

I downloaded a zip of the Unity project from github, then copied the unity.c, unity.h and unity_internals.h files into my STM32 project.

Then, just to test, I created a test_main.c file in my project and added this:
```
#include "main.h"
#include "unity.h"

// Optional setup/teardown
void setUp(void) {}
void tearDown(void) {}

// A simple test case
void test_addition_should_work(void) {
    TEST_ASSERT_EQUAL(5, 2 + 3);
    TEST_ASSERT_EQUAL(-1, 2 - 3);
}

// This function will be called from main()
void runUnityTests(void) {
    UNITY_BEGIN();
    RUN_TEST(test_addition_should_work);
    UNITY_END();
}

```

In my main.c file, I added:
```
  // 🔥 Run unit tests
  runUnityTests();
```

Then, when I flashed the STM32, I saw the output in the serial console:
```
../Core/Src/test_main.c:17:test_addition_should_work:PASS

-----------------------
1 Tests 0 Failures 0 Ignored
OK
```

This was a great first step to help get the basics of testing. I will definitely be using Unity for unit tests in my embedded projects going forward.
Next, I want to actually start my own project. I feel like I've done enough basic experiments, and I'm ready to start doing something useful. Look out for more!