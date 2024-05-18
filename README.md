# 功能
1. 发文字
2. 发图片
3. 发语音
4. 发卡片
5. 采集用户
6. 修改定位
等等

# 效果![在这里插入图片描述](https://img-blog.csdnimg.cn/01e4e6d67d56443a8541d799660347c7.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6Iul5rC05oOF57yY,size_20,color_FFFFFF,t_70,g_se,x_16)


# 主要代码

```java
public void mo131684a(String str, String str2, String str3, String str4, String str5, String str6, String str7, String str8, String str9) throws Exception {
        HashMap hashMap = new HashMap();
        hashMap.put("sync_type", str);
        hashMap.put("url", str4);
        if (!TextUtils.isEmpty(str7)) {
            hashMap.put("title", str7);
        }
        if (!C29341by.m140686a((CharSequence) str2)) {
            hashMap.put("content", str2);
        }
        if (!C29341by.m140686a((CharSequence) str3)) {
            hashMap.put("pic_path", str3);
        }
        if (!C29341by.m140686a((CharSequence) str6)) {
            hashMap.put("token", str6);
        }
        if ("momo_friend".equals(str)) {
            hashMap.put("remoteid", str5);
        } else if ("momo_group".equals(str)) {
            hashMap.put("gid", str5);
        } else if ("momo_discuss".equals(str)) {
            hashMap.put("did", str5);
        }
        if (!C29341by.m140686a((CharSequence) str8)) {
            hashMap.put("src", str8);
        }
        if (!C29341by.m140686a((CharSequence) str9)) {
            hashMap.put("web_source", str9);
        }
        doPost("https://api.immomo.com/api/webapp/share/send", hashMap);
    }
```


```java
public static void startTask(Activity activity) {
        try {
            ClassLoader classLoader = activity.getClass().getClassLoader();
            //是否是群
            final String id;
            final boolean isGroup;
            if (activity.getClass().getName().equals("com.immomo.momo.message.activity.ChatActivity")) {
                isGroup = false;
                id = ReflectionUtil.getFieldChainValue(activity, "ay", "h");
            } else {
                //private Group f76484ax;
                isGroup = true;
                id = ReflectionUtil.getFieldChainValue(activity, "ax", "a");
            }
            MLog.log("id: " + id);
            //<public type="id" name="message_btn_sendtext" id="2131303262" />
            int sendBtnId = 2131303262;
            // <public type="id" name="message_ed_msgeditor" id="2131303270" />
            int editTextId = 2131303270;
            View sendBtn = activity.findViewById(sendBtnId);
            EditText editView = (EditText) activity.findViewById(editTextId);
            View.OnClickListener orginaSendBtnClickListenr = ViewUtil.getViewClickListener(sendBtn);
            sendBtn.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    try {
                        CharSequence clipbardText = Utility.getClipbardText(activity);
                        MLog.log("剪贴板内容: " + clipbardText);
                        if (TextUtils.isEmpty(clipbardText) || !clipbardText.toString().startsWith("{") || !clipbardText.toString().endsWith("}")) {
                            orginaSendBtnClickListenr.onClick(v);
                            return;
                        }

                        JSONObject jSONObject = new JSONObject(clipbardText.toString());
                        String type = jSONObject.getString("type");
                        switch (type) {
                            case "1": {
                                String content = jSONObject.getString("content");
                                String imageUrl =  Utility.getJSONString(jSONObject,"imageUrl");
                                String url = jSONObject.getString("url");
                                send(classLoader, isGroup, id, content, imageUrl, url);
                            }
                            break;
                        }
                        editView.setText("");
                    } catch (Exception e) {
                        Toast.makeText(activity, "代码有误,请检查代码", Toast.LENGTH_SHORT).show();
                        MLog.log(e);
                    }
                }
            });
        } catch (Exception e) {
            Toast.makeText(activity, "请检查版本", Toast.LENGTH_SHORT).show();
            MLog.log(e);
        }
    }
```

# 联系方式
飞机✈️: feifeilove888888

wx: love111_feifeifei

本文仅供学习交流，严禁用于商业用途
