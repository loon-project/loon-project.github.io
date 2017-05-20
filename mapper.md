<h1 align="center">Mapper</h1>

> Mapper feature is separated in another package called `loon-data`.
> It's not production-ready.

## 1 What Does a Mapper Do?

Mapper takes care of mapping object to a model which have relationships with other models.
It's usually used in map relational database query result to model.


## 2 ResultMapping

A complex example:
```typescript
class Comment {

    @Property()
    public id: number;

    @Property()
    public postId: number;

    @Property()
    public name: string;

    @Property()
    public comment: string;
}

class Tag {

    @Property()
    public id: number;
}

class Author {

    @Property()
    public id: number;

    @Property()
    public username: string;

    @Property()
    public password: string;

    @Property()
    public email: string;

    @Property()
    public bio: string;

    @Property('favourite_section')
    public favouriteSection: string;
}

class Post {

    @Property()
    public id: number;

    @Property()
    public subject: string;

    public author: Author;

    public comments: Comment[];

    public tags: Tag[];
}

class Blog {

    @Property()
    public id: number;

    @Property()
    public title: string;

    public author: Author;

    public posts: Post[];
}

const AuthorMap: AssociationMap = {
    property: 'author',
    type: Author,
    prefix: 'author_'
};

const BlogMap: DataMap = {

    type: Blog,

    prefix: 'blog_',

    associations: [
        AuthorMap
    ],

    collections: [
        {
            property: 'posts',
            type: Post,
            prefix: 'post_',

            associations: [
                AuthorMap
            ],

            collections: [
                {
                    property: 'comments',
                    type: Comment,
                    prefix: 'comment_'
                },

                {
                    property: 'tags',
                    type: Tag,
                    prefix: 'tag_'
                }
            ]

        }
    ]

};
/**
 * SQL:
 * select(
 *     'B.id as blog_id',
 *     'B.title as blog_title',
 *     'B.author_id as blog_author_id',
 *     'A.id as author_id',
 *     'A.username as author_username',
 *     'A.password as author_password',
 *     'A.email as author_email',
 *     'A.bio as author_bio',
 *     'A.favourite_section as author_favourite_section',
 *     'P.id as post_id',
 *     'P.blog_id as post_blog_id',
 *     'P.author_id as post_author_id',
 *     'P.created_on as post_created_on',
 *     'P.section as post_section',
 *     'P.subject as post_subject',
 *     'P.draft as post_draft',
 *     'P.body as post_body',
 *     'C.id as comment_id',
 *     'C.post_id as comment_post_id',
 *     'C.name as comment_name',
 *     'C.comment as comment_text',
 *     'T.id as tag_id',
 *     'T.name as tag_name'
 *   )
 *   .from('blogs1 as B')
 *   .leftOuterJoin('authors1 as A', 'A.id', 'B.author_id')
 *   .leftOuterJoin('posts1 as P', 'P.blog_id', 'B.id')
 *   .leftOuterJoin('comments1 as C', 'C.post_id', 'P.id')
 *   .leftOuterJoin('posts_tags1 as PT', 'PT.post_id', 'P.id')
 *   .leftOuterJoin('tags1 as T', 'T.id', 'PT.tag_id')
 *   .where({'B.id': id})
 *
 * Tables:
 *   CREATE TABLE `authors1` (
 *     `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
 *     `username` varchar(255) DEFAULT NULL,
 *     `password` varchar(255) DEFAULT NULL,
 *     `email` varchar(255) DEFAULT NULL,
 *     `bio` varchar(255) DEFAULT NULL,
 *     `favourite_section` varchar(255) DEFAULT NULL,
 *     PRIMARY KEY (`id`)
 *   ) ENGINE=InnoDB AUTO_INCREMENT=89 DEFAULT CHARSET=utf8;
 *
 *   CREATE TABLE `blogs1` (
 *     `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
 *     `title` varchar(255) DEFAULT NULL,
 *     `author_id` int(11) DEFAULT NULL,
 *     PRIMARY KEY (`id`)
 *   ) ENGINE=InnoDB AUTO_INCREMENT=45 DEFAULT CHARSET=utf8;

 *   CREATE TABLE `comments1` (
 *     `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
 *     `post_id` int(11) DEFAULT NULL,
 *     `name` varchar(255) DEFAULT NULL,
 *     `comment` varchar(255) DEFAULT NULL,
 *     PRIMARY KEY (`id`)
 *   ) ENGINE=InnoDB AUTO_INCREMENT=133 DEFAULT CHARSET=utf8;

 *   CREATE TABLE `posts_tags1` (
 *     `post_id` int(11) DEFAULT NULL,
 *     `tag_id` int(11) DEFAULT NULL
 *   ) ENGINE=InnoDB DEFAULT CHARSET=utf8;

 *   CREATE TABLE `posts1` (
 *     `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
 *     `blog_id` int(11) DEFAULT NULL,
 *     `author_id` int(11) DEFAULT NULL,
 *     `created_on` varchar(255) DEFAULT NULL,
 *     `section` varchar(255) DEFAULT NULL,
 *     `subject` varchar(255) DEFAULT NULL,
 *     `draft` tinyint(1) DEFAULT NULL,
 *     `body` varchar(255) DEFAULT NULL,
 *     PRIMARY KEY (`id`)
 *   ) ENGINE=InnoDB AUTO_INCREMENT=89 DEFAULT CHARSET=utf8;

 *   CREATE TABLE `tags1` (
 *     `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
 *     `name` varchar(255) DEFAULT NULL,
 *     PRIMARY KEY (`id`)
 *   ) ENGINE=InnoDB AUTO_INCREMENT=89 DEFAULT CHARSET=utf8;

 *   CREATE TABLE `users1` (
 *     `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
 *     `username` varchar(255) DEFAULT NULL,
 *     `hashedPassword` varchar(255) DEFAULT NULL,
 *     PRIMARY KEY (`id`)
 *   ) ENGINE=InnoDB AUTO_INCREMENT=41 DEFAULT CHARSET=utf8;
 */

class BlogRepository {

    @ResultMapping(BlogMap)
    public queryBlog() {
        return [
            {
                "blog_id": 44,
                "blog_title": "great blog",
                "blog_author_id": 87,
                "author_id": 87,
                "author_username": "Jack",
                "author_password": "password",
                "author_email": "jack@gmail.com",
                "author_bio": null,
                "author_favourite_section": "NO.1",
                "post_id": 87,
                "post_blog_id": 44,
                "post_author_id": 87,
                "post_created_on": "2077-07-07",
                "post_section": "s1",
                "post_subject": "first blog",
                "post_draft": 1,
                "post_body": "body 1",
                "comment_id": 130,
                "comment_post_id": 87,
                "comment_name": "1",
                "comment_text": "c1",
                "tag_id": 87,
                "tag_name": "great"
            },
            {
                "blog_id": 44,
                "blog_title": "great blog",
                "blog_author_id": 87,
                "author_id": 87,
                "author_username": "Jack",
                "author_password": "password",
                "author_email": "jack@gmail.com",
                "author_bio": null,
                "author_favourite_section": "NO.1",
                "post_id": 87,
                "post_blog_id": 44,
                "post_author_id": 87,
                "post_created_on": "2077-07-07",
                "post_section": "s1",
                "post_subject": "first blog",
                "post_draft": 1,
                "post_body": "body 1",
                "comment_id": 131,
                "comment_post_id": 87,
                "comment_name": "2",
                "comment_text": "c2",
                "tag_id": 87,
                "tag_name": "great"
            },
            {
                "blog_id": 44,
                "blog_title": "great blog",
                "blog_author_id": 87,
                "author_id": 87,
                "author_username": "Jack",
                "author_password": "password",
                "author_email": "jack@gmail.com",
                "author_bio": null,
                "author_favourite_section": "NO.1",
                "post_id": 87,
                "post_blog_id": 44,
                "post_author_id": 87,
                "post_created_on": "2077-07-07",
                "post_section": "s1",
                "post_subject": "first blog",
                "post_draft": 1,
                "post_body": "body 1",
                "comment_id": 132,
                "comment_post_id": 87,
                "comment_name": "3",
                "comment_text": "c3",
                "tag_id": 87,
                "tag_name": "great"
            },
            {
                "blog_id": 44,
                "blog_title": "great blog",
                "blog_author_id": 87,
                "author_id": 87,
                "author_username": "Jack",
                "author_password": "password",
                "author_email": "jack@gmail.com",
                "author_bio": null,
                "author_favourite_section": "NO.1",
                "post_id": 87,
                "post_blog_id": 44,
                "post_author_id": 87,
                "post_created_on": "2077-07-07",
                "post_section": "s1",
                "post_subject": "first blog",
                "post_draft": 1,
                "post_body": "body 1",
                "comment_id": 130,
                "comment_post_id": 87,
                "comment_name": "1",
                "comment_text": "c1",
                "tag_id": 88,
                "tag_name": "cloud"
            },
            {
                "blog_id": 44,
                "blog_title": "great blog",
                "blog_author_id": 87,
                "author_id": 87,
                "author_username": "Jack",
                "author_password": "password",
                "author_email": "jack@gmail.com",
                "author_bio": null,
                "author_favourite_section": "NO.1",
                "post_id": 87,
                "post_blog_id": 44,
                "post_author_id": 87,
                "post_created_on": "2077-07-07",
                "post_section": "s1",
                "post_subject": "first blog",
                "post_draft": 1,
                "post_body": "body 1",
                "comment_id": 131,
                "comment_post_id": 87,
                "comment_name": "2",
                "comment_text": "c2",
                "tag_id": 88,
                "tag_name": "cloud"
            },
            {
                "blog_id": 44,
                "blog_title": "great blog",
                "blog_author_id": 87,
                "author_id": 87,
                "author_username": "Jack",
                "author_password": "password",
                "author_email": "jack@gmail.com",
                "author_bio": null,
                "author_favourite_section": "NO.1",
                "post_id": 87,
                "post_blog_id": 44,
                "post_author_id": 87,
                "post_created_on": "2077-07-07",
                "post_section": "s1",
                "post_subject": "first blog",
                "post_draft": 1,
                "post_body": "body 1",
                "comment_id": 132,
                "comment_post_id": 87,
                "comment_name": "3",
                "comment_text": "c3",
                "tag_id": 88,
                "tag_name": "cloud"
            },
            {
                "blog_id": 44,
                "blog_title": "great blog",
                "blog_author_id": 87,
                "author_id": 87,
                "author_username": "Jack",
                "author_password": "password",
                "author_email": "jack@gmail.com",
                "author_bio": null,
                "author_favourite_section": "NO.1",
                "post_id": 88,
                "post_blog_id": 44,
                "post_author_id": 87,
                "post_created_on": "2077-07-17",
                "post_section": "s2",
                "post_subject": "second blog",
                "post_draft": 0,
                "post_body": "body 2",
                "comment_id": null,
                "comment_post_id": null,
                "comment_name": null,
                "comment_text": null,
                "tag_id": 87,
                "tag_name": "great"
            }
        ];
    }
}

const repo = new BlogRepository();
const blog = repo.queryBlog();

blog instanceof Blog # => true
```

In this example, `Blog` have one to many relationship with `Post`, and have one to one relationship with `Author`.
`Post` have one to one relationship with `Author`, and have one to many relationship with `Tag` and `Comment`.

`ResultMapping` will return a single model result, it accept argument with interface `DataMap`. <br>
`DataMap` have one required field is `type`, which specified the model you want to map. <br>
`prefix` is for preventing complex data property name conflict. <br>
`associations` is for the model `to one` relationship.
It accepts array of `AssociationMap`, which extends `DataMap` and have another one required field `property`. <br>
`collections` is for the model `to many` relationship. <br>
It accepts array of `CollectionMap`, which is the same as `AssociationMap`.

> Currently the DataMap should have one property called id to distinguish the different results.


## 3 ResultsMapping

`ResultsMapping` is just like `ResultMapping`, but it returns an array of result.


## 4 EntityMapping

```typescript
class Developer {

    @Property()
    public name: string;

    @Property({name: "skillsContent"})
    public skills: string[];
}

class PeopleRepository {

    @EntityMapping()
    public insertDeveloper(developer: Developer) {
        return developer; // developer is now an object
    }

}
```

The class will automatically convert back to object with `@EntityMapping`.
