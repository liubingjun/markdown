---
title: BaseUtil-Android
date: 2016-03-27 14:13:07
tags: Android
---
# Android Base工具类
## 清空文本
```java
	/**
	 * 用于清除控件内容
	 * @param views 需要清除内容的控件
	 */
	protected void clearText(View... views) {
		for (View v : views) {
			if (v instanceof EditText) {
				((EditText) v).setText("");
			}else if(v instanceof TextView){
				((TextView) v).setText("");
			}
		}
	}
```
## 隐藏软键盘
```java
	/**
	 * 用于屏蔽软键盘
	 * @param views 需要屏蔽软键盘的控件
	 */
	protected void hideKeyBoard(View... views) {
		for (View v : views) {
			if (v instanceof EditText) {
				((EditText) v).setInputType(InputType.TYPE_DATETIME_VARIATION_NORMAL);
			}
		}
	}
```
## 获取焦点
	getCurrentFocus();