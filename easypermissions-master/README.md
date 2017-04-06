# EasyPermissions
This project is forked from EasyPermission, modified some few codes to make it 
more easier to use, which lets you more focus on your business logic.

[中文文档](README-cn.md)

## Feature
- deleted the annotation,only using the CallBack to deal with the permissions callback.
- use a way like PermissionGen to request the permission, looking more clearly


## Usage

This sample is a standard template sample.
Whatever your Android.SDK version is, you can write code like this.

```java
/*
    1. you should implement the EasyPermission.PermissionCallback interface.
*/
public class MainFragment extends Fragment implements EasyPermission.PermissionCallback {
      @Override 
      public void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        /*
            request permissions like PermissionGen which looks more clearly 
        */
        EasyPermission.with(this)
            .addRequestCode(RC_SMS_PERM)
            .permissions(Manifest.permission.READ_SMS)
            .rationale(getString(R.string.rationale_sms))
            .request();
    }

    @Override 
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions,
            @NonNull int[] grantResults) {
        /*
            pass the OnRequestPermissionsResult to EasyPermission
        */
        EasyPermission.onRequestPermissionsResult(this, requestCode, permissions, grantResults);
    }

    @Override 
    public void onPermissionGranted(int requestCode, List<String> perms) {
        /*
            all permissions are granted, continue doing your business here
        */
        Toast.makeText(getActivity(), "TODO: SMS Granted", Toast.LENGTH_SHORT).show();
    }

    @Override 
    public void onPermissionDenied(int requestCode, List<String> perms) {
        /*
            some permissions denied, toast a info to user 
        */
        Toast.makeText(getActivity(), "TODO: SMS Denied", Toast.LENGTH_SHORT).show();

        // optional, go to the Settings Screen.
        EasyPermission.checkDeniedPermissionsNeverAskAgain(this, "cannot work without permissions", perms);
    }

    @Override 
    public void onActivityResult(int requestCode, int resultCode, Intent data) {
        /*
            backing from Settings Activity, these are standard codes 
        */
        if (requestCode == EasyPermission.SETTINGS_REQ_CODE) {
            if(EasyPermission.hasPermissions(getContext(), Manifest.permission.READ_SMS)){
                // all permissions granted
                // do you business...
            }else{
                // some permissions denied
               Toast.makeText(getContext(),"no permission, can not work",Toast.LENGTH_SHORT).show();
            }
        }
    }
}

```

