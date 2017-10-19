

Q: In the RecyclerView, how do we unbind the ViewHolder?

A: You don't need to. Only Fragments need to in onDestroyView().



The reason being that

[ViewHolders] don't outlive the associated view. A Fragment does.
