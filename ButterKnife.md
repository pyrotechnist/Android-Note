

## Unbind

Q: In the RecyclerView, how do we unbind the ViewHolder?

A: You don't need to. Only Fragments need to in onDestroyView().

The reason being that

[ViewHolders] don't outlive the associated view. A Fragment does.


## Conflict with Dagger2

Work around :

```
    // Butter Knife
    compile 'com.jakewharton:butterknife:8.4.0'
    apt 'com.jakewharton:butterknife-compiler:8.4.0'
    // Dagger 2
    apt 'com.google.dagger:dagger-compiler:2.8'
    compile 'com.google.dagger:dagger:2.8'
    provided 'javax.annotation:jsr250-api:1.0'
```
