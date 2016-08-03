##  Android 基于Message的进程间通信


#### 博文：[http://blog.csdn.net/lmj623565791/article/details/47017485](http://blog.csdn.net/lmj623565791/article/details/47017485)


> 安卓5.0以后，谷歌不允许使用隐式意图开启或绑定Service（跨进程）。下面的方法，可以将一个隐式意图转化为显式意图：

```
public static Intent getExplicitIntent(Context context, Intent implicitIntent) {

        // Retrieve all services that can match the given intent
        PackageManager pm = context.getPackageManager();
        List<ResolveInfo> resolveInfo = pm.queryIntentServices(implicitIntent, 0);
        // Make sure only one match was found
        if (resolveInfo == null || resolveInfo.size() != 1) {
            return null;
        }
        // Get component info and create ComponentName
        ResolveInfo serviceInfo = resolveInfo.get(0);
        String packageName = serviceInfo.serviceInfo.packageName;
        String className = serviceInfo.serviceInfo.name;
        ComponentName component = new ComponentName(packageName, className);
        // Create a new intent. Use the old one for extras and such reuse
        Intent explicitIntent = new Intent(implicitIntent);
        // Set the component to be explicit
        explicitIntent.setComponent(component);
        return explicitIntent;
    }
```
