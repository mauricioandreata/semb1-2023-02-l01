PROGNAME = blinky

CC = arm-none-eabi-gcc
LD = arm-none-eabi-gcc
CP = arm-none-eabi-objcopy
RM = rm -rf

OBJDIR = build
DEPDIR = .deps

CFLAGS = -mcpu=cortex-m4 -mthumb -O0 -Wall
DEPFLAGS = -MMD -MP -MF $(DEPDIR)/$*.d
LFLAGS = -nostdlib -T stm32f411-rom.ld -Wl,-Map=blinky.map

SRCS = startup.c \
	   main.c

OBJS = $(patsubst %.c, $(OBJDIR)/%.o, $(SRCS))
$(shell mkdir -p $(dir $(OBJS)) > /dev/null)

DEPS = $(patsubst %.c, $(DEPDIR)/%.d, $(SRCS))
$(shell mkdir -p $(dir $(DEPS)) > /dev/null)

all: $(PROGNAME).bin

$(PROGNAME).bin: $(PROGNAME).elf
	$(CP) -O binary $^ $@

$(PROGNAME).elf: $(OBJS)
	$(LD) $(LFLAGS) -o $@ $^

$(OBJDIR)/%.o:%.c
	 $(CC) -c $(CFLAGS) $(DEPFLAGS) $< -o $@

$(DEPS):

-include $(DEPS)

clean:
	$(RM) $(OBJDIR) $(DEPDIR)
	$(RM) $(PROGNAME).elf $(PROGNAME).bin $(PROGNAME).map