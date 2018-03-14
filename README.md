# BottomNavigation-

##### 1. Create a new project in Android Studio from File ⇒ New Project and select Basic Activity from templates.
##### 2. Download this [res](https://api.androidhive.info/res/bottom_navigation/res.zip) folder and add the drawables to your project’s res. This folder contains necessary drawables required for bottom navigation items.
##### 3. Make sure you have design support library in your build.gradle.
```xml
dependencies {
    implementation 'com.android.support:design:26.1.0'
}
```
##### 4. Add below color, string values to your colors.xml and strings.xml.
colors.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <color name="colorPrimary">#7b4bff</color>
    <color name="colorPrimaryDark">#6539ba</color>
    <color name="colorAccent">#FF4081</color>
    <color name="bgBottomNavigation">#fe485a</color>
</resources>
````
strings.xml
```xml
<resources>
    <string name="app_name">Bottom Navigation</string>
    <string name="title_shop">Shop</string>
    <string name="title_gifts">Gifts</string>
    <string name="title_cart">Cart</string>
    <string name="title_profile">Profile</string>
</resources>
```
##### 5. As the Bottom Navigation items rendered using a menu file, create a new xml named navigation.xml under res ⇒ menu folder.
```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">

    <item
        android:id="@+id/navigation_shop"
        android:icon="@drawable/ic_store_white_24dp"
        android:title="@string/title_shop" />

    <item
        android:id="@+id/navigation_gifts"
        android:icon="@drawable/ic_card_giftcard_white_24dp"
        android:title="@string/title_gifts" />

    <item
        android:id="@+id/navigation_cart"
        android:icon="@drawable/ic_shopping_cart_white_24dp"
        android:title="@string/title_cart" />

    <item
        android:id="@+id/navigation_profile"
        android:icon="@drawable/ic_person_white_24dp"
        android:title="@string/title_profile" />

</menu>
```
##### 6. Open the layout file of main activity i.e activity_main.xml and add BottomNavigationView widget. Here we are also adding a FrameLayout to load the Fragments when the navigation item is selected.
```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.design.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/container"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="info.androidhive.bottomnavigation.MainActivity">

    <FrameLayout
        android:id="@+id/frame_container"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_behavior="@string/appbar_scrolling_view_behavior" />

    <android.support.design.widget.BottomNavigationView
        android:id="@+id/navigation"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_gravity="bottom"
        android:background="?android:attr/windowBackground"
        app:itemBackground="@color/bgBottomNavigation"
        android:foreground="?attr/selectableItemBackground"
        app:itemIconTint="@android:color/white"
        app:itemTextColor="@android:color/white"
        app:menu="@menu/navigation" />

</android.support.design.widget.CoordinatorLayout>
```
##### 7. Now open MainActivity.java and modify it as below.

> Here, OnNavigationItemSelectedListener will be called when the bottom navigation item is selected. For now we are just changing the toolbar title upon selecting the navigation item.

```java
public class MainActivity extends AppCompatActivity {

    private ActionBar toolbar;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        toolbar = getSupportActionBar();

        BottomNavigationView navigation = (BottomNavigationView) findViewById(R.id.navigation);
        navigation.setOnNavigationItemSelectedListener(mOnNavigationItemSelectedListener);

        toolbar.setTitle("Shop");
    }

    private BottomNavigationView.OnNavigationItemSelectedListener mOnNavigationItemSelectedListener
            = new BottomNavigationView.OnNavigationItemSelectedListener() {

        @Override
        public boolean onNavigationItemSelected(@NonNull MenuItem item) {
            Fragment fragment;
            switch (item.getItemId()) {
                case R.id.navigation_shop:
                    toolbar.setTitle("Shop");
                    return true;
                case R.id.navigation_gifts:
                    toolbar.setTitle("My Gifts");
                    return true;
                case R.id.navigation_cart:
                    toolbar.setTitle("Cart");
                    return true;
                case R.id.navigation_profile:
                    toolbar.setTitle("Profile");
                    return true;
            }
            return false;
        }
    };
}
```
##### 8. Create new Fragment by going to File ⇒ New ⇒ Fragment ⇒ Fragment (Blank) and name it as StoreFragment.java. Likewise create other three fragments too.
##### 9. Open MainActivity.java and modify bottom navigation listener as below to load the fragments in FrameLayout.

> loadFragment() – loads the Fragment into FrameLayout. The same method is called in OnNavigationItemSelectedListener callback by passing appropriate fragment instance.
> The logic needed for specific module goes into appropriate Fragment keeping the MainActivity clean.

```java
public class MainActivity extends AppCompatActivity {

    private ActionBar toolbar;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        toolbar = getSupportActionBar();

        // load the store fragment by default
        toolbar.setTitle("Shop");
        loadFragment(new StoreFragment());
    }

    private BottomNavigationView.OnNavigationItemSelectedListener mOnNavigationItemSelectedListener
            = new BottomNavigationView.OnNavigationItemSelectedListener() {

        @Override
        public boolean onNavigationItemSelected(@NonNull MenuItem item) {
            Fragment fragment;
            switch (item.getItemId()) {
                case R.id.navigation_shop:
                    toolbar.setTitle("Shop");
                    fragment = new StoreFragment();
                    loadFragment(fragment);
                    return true;
                case R.id.navigation_gifts:
                    toolbar.setTitle("My Gifts");
                    fragment = new GiftsFragment();
                    loadFragment(fragment);
                    return true;
                case R.id.navigation_cart:
                    toolbar.setTitle("Cart");
                    fragment = new CartFragment();
                    loadFragment(fragment);
                    return true;
                case R.id.navigation_profile:
                    toolbar.setTitle("Profile");
                    fragment = new ProfileFragment();
                    loadFragment(fragment);
                    return true;
            }

            return false;
        }
    };

    private void loadFragment(Fragment fragment) {
        // load fragment
        FragmentTransaction transaction = getSupportFragmentManager().beginTransaction();
        transaction.replace(R.id.frame_container, fragment);
        transaction.addToBackStack(null);
        transaction.commit();
    }

}
```
