# Inline-JNI
JNI to C++ wrapper which aims to make JNI a little bit more usable

A header-only wrapper for JNI functionality, making it less painful to execute Java code from C++.

**This library heavily uses C++11 user-defined literals, the minimum GCC version to use this is 4.8**

# What is this used for?
Primarily, I have used it for better Android integration from C++. Getting system information or calling Android APIs that are not exposed through the NDK becomes much easier, and the code is much more minimal.

As an example, this is the code to fetch the name of the hardware board on Android:

    auto build = "android.os.Build"_jclass;
    auto field = "BOARD"_jfield;
    auto boardName = build[FieldAs<JavaString>(field)].value();

The syntax is modelled to be close to Java. The API is not perfect, but simplifies some aspects of interacting with JNI from C++.

There is also support for calling functions on class instances, but this is not heavily tested.

# How do I use this?

Most of this library is header-only, but, for obvious reasons, it needs access to the JNI environment in order to stay safe.

The following function must be defined:

    namespace JNIPP {
    
    JNIEnv* GetJNI()
    {
        ...
    }
    
    }
    
Without this, it would become considerably less fun to use, as you would have to guarantee that the JNI is current to the thread.

Also, on that note, **this library is not made to be thread-safe**.
