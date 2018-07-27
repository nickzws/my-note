**前言：**

测试部分！

1. sad
2. 2ewae
3. 3
4. 敖德萨多

> 大厦大厦



```php
<?php

namespace api\modules\v1\models\activity;

use common\models\service\activity\ActivityComment;
use common\models\service\activity\ActivityCommentLike;
use Yii;
use api\modules\v1\models\user\User;

class CommentLike extends ActivityCommentLike
{
    public function fields()
    {
        $fields = parent::fields();


        return $fields;
    }


    //增加评论的点赞数
    public static function updateCommentLikeCount($comment_id)
    {
        if (null !== $obj = ActivityComment::findOne($comment_id)) {
            $obj->updateCounters(['liketimes' => 1]);
        }

    }

    //减去评论的点赞数
    public static function delCommentLikeCount($comment_id)
    {
        return ActivityComment::findOne($comment_id)->updateCounters(['liketimes' => -1]);
    }

    //评论点赞
    public static function likes(int $comment_id)
    {
        $liked = static::findOne(['cid' => $comment_id, 'uid' => \Yii::$app->user->id,]);

        //若已点赞则直接返回
        if (true == $liked) {
            return $liked;
        }

        $model = new static;
        $model->attributes = ['cid' => $comment_id, 'uid' => \Yii::$app->user->id,];

        $model->save();
        static::updateCommentLikeCount($comment_id);
        return $model;
    }

    public static function like(int $comment_id)
    {
        $liked = static::findOne(['cid' => $comment_id, 'uid' => \Yii::$app->user->id]);

        //若已点赞则直接返回
        if (true == $liked) {
            return $liked;
        }

        $model = new static;
        $model->attributes = ['cid' => $comment_id, 'uid' => \Yii::$app->user->id];

        $model->save();
        static::updateCommentLikeCount($comment_id);
        return $model;
    }

    //评论取消点赞
    public static function unlike($comment_id)
    {
        $liked = static::findOne(['cid' => $comment_id, 'uid' => \Yii::$app->user->id]);
        if (true == $liked) {
            $liked->delete();
            static::delCommentLikeCount($comment_id);//减去点赞数
            return true;
        }
        return false;
    }
}
```

```
  这是我写的第一本gotbook，一直在找比较好的markdown来记录学习的点点滴滴，现在终于实现了！![](/assets/7.jpg)
```

---



