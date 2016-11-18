title: java枚举类型的使用
id: 1689
categories:
  - Java
date: 2015-06-08 14:42:45
tags:
---

    public enum NodeType {
    TYPE_BEGIN("是否区分上下班", 0),
    TYPE_WORK_REST("区分上下班[上下班时间]", 1),
    TYPE_TO_EXTENSION("提示用户拨分机号", 2),
    TYPE_SPECIFY_KEY("进入下级菜单", 3),
    TYPE_CALL_MANUAL_SERVICE("呼叫指定分机", 4),
    TYPE_LEAVE_MESSAGE("语音提示", 5),
    TYPE_REPLAY_VOICE("重放语音提示", 6),
    TYPE_AREA_ROUTE("区域路由", 7),
    TYPE_RETURN("返回上一级", 8),
    TYPE_RETURN_TOP("返回第一级", 9);

    int type;
    String name;

    NodeType(String name, int type) {
        this.name = name;
        this.type = type;
    }

    public static NodeType valueOf(Integer type) {
        for (NodeType nodeType : NodeType.values()) {
            if (type.equals(nodeType.getType())) {
                return nodeType;
            }
        }
        return null;
    }

    public int getType() {
        return type;
    }

    public String getName() {
        return name;
    }

    public String toString() {
        return name;
    }

    }
    
