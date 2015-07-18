chordid~

This external implements the Chromagram/ChordDetector by Adam Stark.
(https://github.com/adamstark/Chord-Detector-and-Chromagram)

I have extensively modified the ChordDetector code to change the classification
scheme, and have extended the range of detectable chords. This may create
errors of classification - please let me know if you find any.

My thanks also to Fabrizio Poce (J74 Harmotools), who reviewed my initial 
VST implemention. This external is much better, I think. I'm glad I came back to it.
http://fabriziopoce.com/HarmoTools.html

I have also used the maxmpp C++ template as the basis of this external.
https://github.com/grrrwaaa/maxcpp
My thanks to Graham Wakefield for this template - it made my introduction to 
Max/MSP external programming much easier.

Any issues with the code are mine: please contact me if you have queries
or suggestions for improvement.

It has a single signal inlet/outlet acting as a passthrough.
inlet messages:
  bang 
    resend last chord data on data outlets
  rate <int>
    this sets the number of 512 sample frames the Chromagram detector waits before 
    generating a new chromagram. Default is 1 for the most rapid response. Increase this to
    reduce CPU use.
  rms <float>
    this sets a cutoff volume to prevent noise generating random chords.

outlet messages
  0 : signal
    passthrough
    
  1 : chord name
    text symbol with the chord name
    
  2 : chord number
    this is a 12-bit chord type, plus the root_note value * 10000
    root_note values use C = 0
    the chord type is C rooted chord ...
            C  C# D  D# E  F  F# G  G# A  A# B
      Major 1  0  0  0  1  0  0  1  0  0  0  0 = 2192
      Minor 1  0  0  1  0  0  0  1  0  0  0  0 = 2320
      7th   1  0  0  0  1  0  0  1  0  0  1  0 = 2194
      and so on ...
    e.g
    D7th would be root_note D = 2, * 10000 = 20000, + 2194 = 22194
    Em            root_note E = 4, * 10000 = 40000, + 2320 = 42320
    
    It is relatively easy to extract suitable information from this format.
    Some enharmonic chords may be incorrectly classified 
      e.g Dsus4 is enharmonic with Gsus2
    This cannot yet be avoided, but I have some future ideas.

  3 : list of MIDI note values from middle C (60)
    the note values produce a chord that is the same as the detected chord
    
  4 : rms_cutoff
    this is 0 if the input is below the cutoff value, and 1 if the chord detector is 
    active on the input stream.
    
Licence:

 * This program is free software: you can redistribute it and/or modify
 * it under the terms of the GNU General Public License as published by
 * the Free Software Foundation, either version 3 of the License, or
 * (at your option) any later version.
 *
 * This program is distributed in the hope that it will be useful,
 * but WITHOUT ANY WARRANTY; without even the implied warranty of
 * MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
 * GNU General Public License for more details.
 *
 * You should have received a copy of the GNU General Public License
 * along with this program.  If not, see <http://www.gnu.org/licenses/>.
