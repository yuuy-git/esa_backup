---
title: "tensorflow_closedsessionerror"
category: Users/kz
tags: 
created_at: 2018-08-15 05:55:14 +0900
updated_at: 2018-08-15 06:06:36 +0900
published: false
number: 18
---

# tensorflow error
* 起きたこと
tensorflowでmultiplyとmatmulの違いを実装で確かめようと下記codeを入力し、実行したらRuntimeerrorが起きた。どうやら、sessionが閉じられてしまっているらしい。

*問題点 
Session as sessのつもりがSession as sees になっていた。

```python
x = tf.constant([[1,2],[3,4]], name='const3')
y = tf.constant([[1,2],[3,4]], name='const4')

mul = tf.multiply(x,y)
mul2 = tf.matmul(x,y)

#init = tf.initialize_all_variables()


with tf.Session() as sees:
#    sess.run(init)
    print(sess.run(mul))
    print(sess.run(mul2))

```

```
---------------------------------------------------------------------------
RuntimeError                              Traceback (most recent call last)
<ipython-input-22-c10ee2ee9504> in <module>()
     10 with tf.Session() as sees:
     11 #    sess.run(init)
---> 12     print(sess.run(mul))
     13     print(sess.run(mul2))

/Users/Kazuki/anaconda/lib/python3.6/site-packages/tensorflow/python/client/session.py in run(self, fetches, feed_dict, options, run_metadata)
    898     try:
    899       result = self._run(None, fetches, feed_dict, options_ptr,
--> 900                          run_metadata_ptr)
    901       if run_metadata:
    902         proto_data = tf_session.TF_GetBuffer(run_metadata_ptr)

/Users/Kazuki/anaconda/lib/python3.6/site-packages/tensorflow/python/client/session.py in _run(self, handle, fetches, feed_dict, options, run_metadata)
   1056     # Check session.
   1057     if self._closed:
-> 1058       raise RuntimeError('Attempted to use a closed Session.')
   1059     if self.graph.version == 0:
   1060       raise RuntimeError('The Session graph is empty.  Add operations to the '

RuntimeError: Attempted to use a closed Session.

```

