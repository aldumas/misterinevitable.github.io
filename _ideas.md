For the main navigation, when viewing on a large screen, have
this code replace the navigation options. To get the syntax highlighting,
temporarily create a post with this as a code block and copy the source
that's generated. Either the case line or the full case block are clickable.

void on_nav_click(navitem_t selection)
{
    void (*fun)();

    switch (selection) {

    case HOME:
    default:
        fun = show_home;
        break;

    case CATEGORIES:
        fun = show_categories;
        break;
    
    case TAGS:
        fun = show_tags;
        break;

    case ARCHIVES:
        fun = show_archives;
        break;

    case ABOUT:
        fun = show_about;
        break;

    }

    navigate(fun);
}