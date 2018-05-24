title: Android 打印当前行数和名称方法
author: 大帅
tags:
  - log
categories:
  - Android
date: 2018-05-24 16:12:00
---
### 获取类名称 行数
		public static String getFunctionName() {
        StackTraceElement[] sts = Thread.currentThread().getStackTrace();

        if (sts == null) {
            return null;
        }

        for (StackTraceElement st : sts) {
            if (st.isNativeMethod()) {
                continue;
            }

            if (st.getClassName().equals(Thread.class.getName())) {
                continue;
            }

            if (st.getClassName().equals(Logger.class.getName())) {
                continue;
            }

            return "[" + Thread.currentThread().getName() + "("
                    + Thread.currentThread().getId() + "): " + st.getFileName()
                    + ":" + st.getLineNumber() + "]";
        }

        return null;
    }
### 打印超多数字
    public static void i(String msg) {
        if (debug) {
            String message = createMessage(msg);
            if (message.length() > 4000) {
                int chunkCount = message.length() / 4000;     // integer division
                for (int i = 0; i <= chunkCount; i++) {
                    int max = 4000 * (i + 1);
                    if (max >= message.length()) {
                        Log.i(tag,message.substring(4000 * i)+"\n");
                    } else {
                        Log.v(tag, message.substring(4000 * i, max)+"\n");
                    }
                }
            }else {
                Log.i(tag, message);
            }
        }
    }