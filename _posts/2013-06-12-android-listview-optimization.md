---
layout: post
title: Android ListView Optimization
---

When building a ListView in Android, 9 times out of 10 it's likely that you're using a custom Adapter to bind data to your list.  By introducing the ViewHolder pattern into your Adapter, you can increase the performance of your ListView substantially.  

Let's take the following sample code:

{% highlight java %}
public class MyAdapter extends ArrayAdapter {

    // skipping the constructor for brevity

    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        // create the view holder
        MyViewHolder holder;

        if (convertView == null) {
            // mContext is the context I passed into the constructor
            LayoutInflater inflater = (LayoutInflater) mContext.getSystemService(Context.LAYOUT_INFLATER_SERVICE);
            convertView = inflater.inflate(R.layout.your_layout_here, null);

            holder = new MyViewHolder();
            holder.title = (TextView) convertView.findViewById(R.id.some_id_here);

            convertView.setTag(holder);
        }
        else {
            holder = (MyViewHolder) convertView.getTag();
        }

        holder.title.setText("What up");
    }

    private class MyViewHolder {
        public TextView title;
    }

}
{% endhighlight %}

<em>findViewById()</em> can be very expensive, especially if you have to perform it multiple times.  As you scroll through a large ListView, this can be a large performance hit.  If we only perform the <em>findViewById()</em> once and associate it with the convertView, on subsequent rendering of that ListView item we won't have to perform the <em>findViewById()</em>.  Everyone wins, especially your users who don't have to put up with laggy ListViews.