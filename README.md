# Launchpad Pro
Open source firmware for the Novation Launchpad Pro grid controller

# Philosophy
We could have released the full source for the factory shipping firmware, but we've decided not to for a variety of reasons.  Instead, we created a simplified framework for developing "apps" on Launchpad, which comprises a build environment, application entry points / API, and a library of low level source code.  Our reasoning is as follows:

- There is no value in customising low level routines such as LED multiplexing or ADC scanning - this code has been carefully tweaked over many months to deliver the best results, and is not something you'd want to tweak.  I'm prepared to be convinced otherwise!
- There is very little value in customising main() or other low level features, and again these things are hard to do well.
- If we shipped the application firmware as-is, we'd have a support nightmare on our hands (imagine the support calls - my "Launchpad Pro is behaving strangely...").  Instead, we wanted to create a clear boundary between "normal" usage with Ableton, and custom firmware.  As such, Ableton integration has been removed from this firmware, as has the setup / navigation functionality.
- If we left the Ableton integration and menu structure in place, open firmware developers would have to work around it.  They would also potentially consume precious RAM/CPU resources.  I've a feeling this isn't what you'd want, but again, we're interested to hear your feedback.
- Licensing requirements for the CMSIS library version we use are ambiguous.  Yes, we could port to the public version, but why bother, given the above reasoning - I'd prefer to spend my time on good documentation and examples.  As such, all the CMSIS code is compiled into launchpad_pro.a, and we do not need to distribute the headers.

I'm sure you'll have feedback for us, so please do get in touch!  I'm [blogging the process too](http://dvhdr.tumblr.com/) if you'd like to read my musings.

# Installation
This project uses [Vagrant](https://www.vagrantup.com/) to manage the build environment. As such, you need to:

1. Install [Vagrant](https://www.vagrantup.com/)
2. Install [VirtualBox](https://www.virtualbox.org/wiki/Downloads)(or another virtualisation platform of your choice)
3. `vagrant up`

# Building
SSH into the Vagrant "box" by doing `vagrant up`
At the command prompt, simply type `make`

This will generate the firmware launchpad_pro.syx file in the build directory.  You can then upload this to your Launchpad Pro from the host!

# Uploading to a Launchpad Pro
Bootloader firmware update instructions here (Sysex Librarian / MIDI-OX / ...?)

# API
The most crucial parts of the API are:

- Recieving messages from the pads and buttons
- Writing colours to the LEDs
- Sending and receiving messages from the USB and MIDI ports
- Receiving a tick message to drive timing based code

Up for debate:

- Flash read / write access
- Setup button behaviour

# Debugging
We decided not to support or encourage using a JLink or similar for debugging, as opening a Launchpad Pro to fit a debugging header can easily damage the FSR (force sensitive resistor) sheet.

Instead, you're going to have to do things the old fashioned way - by blinking LEDs or sending MIDI messages.  FWIW, that's the way I've developed this version of the firmware - dogfooding all the way ;)

If you want to test code in detail, we suggest developing it on a host (we'd love to release our simulator environment at a later stage, but there are currently no plans to do so).
