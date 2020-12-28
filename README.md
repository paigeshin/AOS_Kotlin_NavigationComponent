- [OfficialDoc](https://developer.android.com/guide/navigation/navigation-getting-started)

# Three Major Components

- Navigation Graph
- NavHostFragment
- NavController

# Add to Gradle

- Application Level Gradle

```kotlin
buildscript {
    repositories {
        google()
    }
    dependencies {
        def nav_version = "2.3.2"
        classpath "androidx.navigation:navigation-safe-args-gradle-plugin:$nav_version"
    }
}
```

- App Level Gradle

```kotlin
dependencies {
  def nav_version = "2.3.2"

  // Java language implementation
  implementation "androidx.navigation:navigation-fragment:$nav_version"
  implementation "androidx.navigation:navigation-ui:$nav_version"

  // Kotlin
  implementation "androidx.navigation:navigation-fragment-ktx:$nav_version"
  implementation "androidx.navigation:navigation-ui-ktx:$nav_version"

  // Feature module Support
  implementation "androidx.navigation:navigation-dynamic-features-fragment:$nav_version"

  // Testing Navigation
  androidTestImplementation "androidx.navigation:navigation-testing:$nav_version"

  // Jetpack Compose Integration
  implementation "androidx.navigation:navigation-compose:1.0.0-alpha04"
}
```

To generate Java language code suitable for Java or mixed Java and Kotlin modules, add this line to **your app or module's** **`build.gradle`** file:

`apply plugin: "androidx.navigation.safeargs"`

Alternatively, to generate Kotlin code suitable for Kotlin-only modules add:

`apply plugin: "androidx.navigation.safeargs.kotlin"`

# Binding without `layoutInflater`

```kotlin
class MainActivity : AppCompatActivity() {

    private lateinit var binding: ActivityMainBinding

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = DataBindingUtil.setContentView(this, R.layout.activity_main)

        
    }

}
```

# Create a new android resource directory on `App` Directory with resource type `navigation`

- Resource Type: `navigation`

⇒ And then you will see `navigation` folder is created on `res` folder.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/081f43a4-3791-4b91-815f-fd553b013391/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/081f43a4-3791-4b91-815f-fd553b013391/Untitled.png)

# Navigation Host Fragment

- Create `NavHost` on HostFragment (activity)

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">

    <androidx.constraintlayout.widget.ConstraintLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".MainActivity">

        <fragment
            android:id="@+id/fragment"
            android:name="androidx.navigation.fragment.NavHostFragment"
            android:layout_width="0dp"
            android:layout_height="0dp"
            app:defaultNavHost="true"
            app:layout_constraintBottom_toBottomOf="parent"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toTopOf="parent"
            app:navGraph="@navigation/nav_graph" />

    </androidx.constraintlayout.widget.ConstraintLayout>

</layout>
```

# Navigation Destinations

### HomeFragment

```kotlin
package com.paigesoftware.navdemo

import android.os.Bundle
import androidx.fragment.app.Fragment
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.databinding.DataBindingUtil
import com.paigesoftware.navdemo.databinding.FragmentHomeBinding

class HomeFragment : Fragment() {

    private lateinit var binding: FragmentHomeBinding

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        // Inflate the layout for this fragment
        binding = DataBindingUtil.inflate(inflater, R.layout.fragment_home, container, false)

        return binding.root
    }

}
```

### SecondFragment

```kotlin
package com.paigesoftware.navdemo

import android.os.Bundle
import androidx.fragment.app.Fragment
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.databinding.DataBindingUtil
import com.paigesoftware.navdemo.databinding.FragmentSecondBinding

class SecondFragment : Fragment() {

    private lateinit var binding: FragmentSecondBinding

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {

        binding = DataBindingUtil.inflate(inflater, R.layout.fragment_second, container, false)

        return binding.root
    }

}
```

# Navigation Actions

- Go to Next Fragment

```kotlin
// Go to next fragment
binding.button.setOnClickListener {
    it.findNavController().navigate(R.id.action_homeFragment_to_secondFragment)
}
```

- Entire Code

```kotlin
package com.paigesoftware.navdemo

import android.os.Bundle
import androidx.fragment.app.Fragment
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.databinding.DataBindingUtil
import androidx.navigation.findNavController
import com.paigesoftware.navdemo.databinding.FragmentHomeBinding

class HomeFragment : Fragment() {

    private lateinit var binding: FragmentHomeBinding

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        // Inflate the layout for this fragment
        binding = DataBindingUtil.inflate(inflater, R.layout.fragment_home, container, false)

        // Go to next fragment
        binding.button.setOnClickListener {
            it.findNavController().navigate(R.id.action_homeFragment_to_secondFragment)
        }

        return binding.root
    }

}
```

# Transforming Data Between Destinations

### Send Data

```kotlin
// Go to next fragment
  binding.button.setOnClickListener {
      // Send Data with Bundle
      val bundle: Bundle = bundleOf("user_input" to binding.editText.text.toString())
      it.findNavController().navigate(R.id.action_homeFragment_to_secondFragment, bundle)
  }
```

### Receive Data

```kotlin
//receive data
val input = requireArguments().getString("user_input")
binding.textView.text = input.toString()
```

### Entire Code

- HomeFragment

```kotlin
package com.paigesoftware.navdemo

import android.os.Bundle
import android.text.TextUtils
import androidx.fragment.app.Fragment
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.Toast
import androidx.core.os.bundleOf
import androidx.databinding.DataBindingUtil
import androidx.navigation.findNavController
import com.paigesoftware.navdemo.databinding.FragmentHomeBinding

class HomeFragment : Fragment() {

    private lateinit var binding: FragmentHomeBinding

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View {
        // Inflate the layout for this fragment
        binding = DataBindingUtil.inflate(inflater, R.layout.fragment_home, container, false)

        // Go to next fragment
        binding.button.setOnClickListener {
            // Send Data with Bundle
            if(!TextUtils.isEmpty(binding.editText.text.toString())) {
                val bundle: Bundle = bundleOf("user_input" to binding.editText.text.toString())
                it.findNavController().navigate(R.id.action_homeFragment_to_secondFragment, bundle)
            } else {
                Toast.makeText(activity, "Please insert your name", Toast.LENGTH_LONG).show()
            }

        }

        return binding.root
    }

}
```

- SecondFragment

```kotlin
package com.paigesoftware.navdemo

import android.os.Bundle
import androidx.fragment.app.Fragment
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.databinding.DataBindingUtil
import com.paigesoftware.navdemo.databinding.FragmentSecondBinding

class SecondFragment : Fragment() {

    private lateinit var binding: FragmentSecondBinding

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View {

        binding = DataBindingUtil.inflate(inflater, R.layout.fragment_second, container, false)

        //receive data
        val input = requireArguments().getString("user_input")
        binding.textView.text = input.toString()

        return binding.root
    }

}
```

# Animation For Actions

- On Right Menu, you can find `Animations`
- Apply these animations found on stackoverflow → [https://stackoverflow.com/questions/5151591/android-left-to-right-slide-animation](https://stackoverflow.com/questions/5151591/android-left-to-right-slide-animation)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3613c116-1d5a-4c9a-bfc2-9d63f8feeaba/Screen_Shot_2020-12-28_at_9.42.09_PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3613c116-1d5a-4c9a-bfc2-9d63f8feeaba/Screen_Shot_2020-12-28_at_9.42.09_PM.png)

[Animation Sources](https://www.notion.so/Animation-Sources-40f67e199dda405f9789c31f87159cf8)

### Popular Set of Animations for navigation controller

**anim_slide_in_left.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android" >
    <translate
        android:duration="600"
        android:fromXDelta="100%"
        android:toXDelta="0%" >
    </translate>
</set>

```

**anim_slide_in_right.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android" >
    <translate
        android:duration="600"
        android:fromXDelta="-100%"
        android:toXDelta="0%" >
    </translate>
</set>

```

**anim_slide_out_left.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android" >
    <translate
        android:duration="600"
        android:fromXDelta="0%"
        android:toXDelta="-100%" >
    </translate>
</set>

```

**anim_slide_out_right.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android" >
    <translate
        android:duration="600"
        android:fromXDelta="0%"
        android:toXDelta="100%" >
    </translate>
</set>
```

### Set

- Enter: slide_in_right
- Exit: slide_out_left
- Pop Enter: slide_in_left
- Pop Exit: slide_out_right