<template>
    <Layout>
        <!-- <span style="display: none;" v-html="$context" /> -->
        <div class="content-page">
            <main class="content">
                <section class="url-container">
                    <div class="url-container">
                        <iframe v-if="$context.currentContent.url.includes('youtube')" class="url" width="auto" height="auto" :src="$context.currentContent.url.replace('/watch?v=', '/embed/').replace('youtube', 'youtube-nocookie') + '?autoplay=1&showinfo=0&rel=0&vq=hd1080&enablejsapi=1'" autoplay frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen />
                        <iframe v-else-if="$context.currentContent.url.includes('quizlet')" :src="$context.currentContent.url + '/learn/embed'" class="url" style="border: none;" />
                    </div>
                </section>
                <section class="controls">
                    <div class="course-information">
                        <div class="contents">
                            <g-link v-for="content in $context.class.contents" v-bind:key="content.url" :to="$context.data.classesRoute + $context.class.slug + '/' + parseSlug(content.title) + '/'">
                                <div class="active-url-link" v-if="parseSlug(content.title) == parseSlug($context.currentContent.title)">
                                    {{ $context.class.contents.indexOf(content)+1 }}.  {{ content.title }}
                                    <g-image v-if="content.url.includes('quizlet')" alt="quiz icon" src="~/assets/img/icons/quiz.png" />
                                    <g-image v-else-if="content.url.includes('youtube')" alt="video icon" src="~/assets/img/icons/video.png" />
                                </div>
                                <div v-else>
                                    {{ $context.class.contents.indexOf(content)+1 }}.  {{ content.title }}
                                    <g-image v-if="content.url.includes('quizlet')" src="~/assets/img/icons/quiz.png" />
                                    <g-image v-else-if="content.url.includes('youtube')" src="~/assets/img/icons/video.png" />
                                </div>
                            </g-link>
                        </div>
                    </div>
                </section>
            </main>

            <ClassInfo
                :classes_route="$context.data.classesRoute"
                :title="$context.currentContent.title"
                :description="$context.class.description"
                :contents="$context.class.contents"
                :instructor="$context.class.instructor"
                :date_created="$context.class.date_created"
                :include_contents="false"
            />
        </div>
    </Layout>
</template>

<script>
import ClassInfo from '~/components/ClassInfo.vue'

export default {
    components: {
        ClassInfo
    },
    methods: {
        parseSlug(slug) {
            return slug.toLowerCase().split(' ').join('-').split('(').join('').split(')').join('')
        }
    },
    metaInfo() {
        return {
            title: this.$context.currentContent.title,
            meta: [
                {
                    name: 'description',
                    content: this.$context.class.description.length > 165 ? this.$context.class.description.substring(0, 162) + '...' : this.$context.class.description
                },
                {
                    name: 'author',
                    content: this.$context.class.instructor
                }
            ]
        }
    }
}
</script>

<style lang="scss"> 
html {
    background: $white;
}

main.content {
    width: 80%;
    max-height: 67.5vh;
    padding: 1rem 10%;
    padding-top: 4rem;
    padding-bottom: 0rem;
    // overflow: hidden;
    background: $white;
    display: grid;
    grid-gap: 2rem;
    grid-template-columns: 70% auto;

    @include tablet {
        grid-template-columns: 1fr;
        max-height: 100%;
    }

    @include phone {
        width: 100%;
        padding: 1rem 0;
        padding-top: 0;
        grid-gap: 0;
        max-height: 110%;
    }

    .url-container {
        display: flex;
        flex-direction: column;
        width: 100%;
        height: 100%;

        @include phone {
            padding-top: .5rem;
            background: #fff;
        }

        .url-container {
            position: relative; padding-bottom: 56.25%; padding-top: 30px; height: 0; overflow: hidden;

            iframe {
                position: absolute; top: 0; left: 0; width: 100%; height: 99%;
            }
        }
    }

    .controls {
        max-height: 100%;

        .contents {
            background: #fff;
            border-radius: .25rem;
            max-height: 67.5vh;
            overflow-y: scroll;
            overflow-x: hidden;

            @include tablet {
                max-height: 150vh;
                overflow: auto;
                overflow-x: hidden;
            }
            
            a {
                color: inherit;
                background: #fff;
                display: block;
                width: 100%;
                border-radius: .25rem;
                text-decoration: none;
                border: solid 1px #fff;
                transition: 200ms ease;

                img {
                    position: relative;
                    width: 1.1rem;
                    height: 1.1rem;
                    top: .175rem;
                    margin-left: .2rem;
                }
                
                div {
                    opacity: .8;
                    padding: 1.4rem 1.25rem;
                }

                div.active-url-link {
                    opacity: .95;
                    color: $light-blue;
                    background: rgb(243, 243, 243);
                }

                div.completed-content {
                    text-decoration: strikethrough;
                }

                &:hover {
                    opacity: 1;
                    border-top: solid 1px rgba(230, 230, 230, .9);
                    border-bottom: solid 1px rgba(230, 230, 230, .9);
                }
            }
        }
    }
}

.content-page .class-info {
    width: 80% !important;
    margin: 0 auto;

    h1 {
        font-size: 3rem !important;
        margin: 0;
    }

    h3 {
        font-size: 2rem !important;
    }
}
</style>
