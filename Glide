
   #  Need support lib 
    
    compile 'com.github.bumptech.glide:glide:3.8.0'
    compile 'com.android.support:support-v4:25.0.0'
    
    
    # SimpleTarget 
    
    ```
    final ImageView  imageViewPreview = (ImageView) view.findViewById(R.id.image_preview);

            Image image = images.get(position);

            Glide.with(getActivity())
                    .load(image.getLarge())
                    .asBitmap()
            .into(new SimpleTarget<Bitmap>() {
                @Override
                public void onResourceReady(Bitmap bitmap, GlideAnimation anim) {
                    imageViewPreview.setImageBitmap(bitmap);
              
                }
            });
    ```
    
