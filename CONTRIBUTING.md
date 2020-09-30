Conditions in main in wg.c can be optimised.
```
int main(int argc, char *argv[])
{
        PROG_NAME = argv[0];
        if (argc == 2) { // the two conditions bellow depends on this one
                if (!strcmp(argv[1], "-v") || !strcmp(argv[1], "--version") || !strcmp(argv[1], "version"))) {
                        printf("wireguard-tools v%s - https://git.zx2c4.com/wireguard-tools/\n",  WIREGUARD_TOOLS_VERSION);
                        return 0;
                }
                if ((!strcmp(argv[1], "-h") || !strcmp(argv[1], "--help") || !strcmp(argv[1], "help"))) {
                        show_usage(stdout);
                        return 0;
                }
        }
        else if (argc == 1) { // don't need to compare if there is aleady 2 args
                static char *new_argv[] = { "show", NULL };
                return show_main(1, new_argv);
        }
        for (size_t i = 0; i < sizeof(subcommands) / sizeof(subcommands[0]); ++i) {
                if (!strcmp(argv[1], subcommands[i].subcommand))
                        return subcommands[i].function(argc - 1, argv + 1);
        }
        fprintf(stderr, "Invalid subcommand: `%s'\n", argv[1]);
        show_usage(stderr);
        return 1;
}
```
