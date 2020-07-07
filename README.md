#CnC_Remastered_Collection

Reading this code is pretty amazing experience, CnC is my one of favoruite game of childhood.
Lets try to keep it more new C++ starads friendly, also checkout Jason Turner video on YT about this code.

Assumptions:
========
The code should compile both on linux and windows platform.
There are some problems ind bugs in the game. I saw that this code can be used to develop mods for the game.


TODO:
========
- FIX pathfinding of the harvesters. Multiple harvesters have problems with dumping in to the same factory. 
- FIX harvaster should turn back to nearest factory
- FIX pathfinding, when moving unit stuck, unit calculate new path, often around the map and its going back intead of attack
- ADD Building queue
- GUI display 'power' level of each player, (singleplayer)
- prepare CMake project

FINDOUT:
========
-- HOW harvesters finds the seeds?
-- HOW they finds the route?
-- HOW they interact with eachother?

Potential problems and capability issues:
========
- Initialization fields by colon ':'.
- Boolean threaded as int.

Hints:
========
in the Save/Load system there is always a palce (buffer) for eventualy additional data to keep game compatible after updates.


Important classes and files:
========

- The GUI system is described in GADGET.H
- LIST.H represents linked list implementation.
- VECTOR.H/CPP impelements template vector class, like this one from STL. DynamicVectorArrayClass, BooleanVectorClass, DynamicVectorClass : VectorClass<T> 


Gameplay
===

/*
**	This class holds the information about the current game being played. This information is
**	global to the scenario and is generally of a similar nature to the information that was held
**	in the controlling scenario INI file. It is safe to write this structure out as a whole since
**	it doesn't contain any embedded pointers.
*/
class ScenarioClass {

EXTERNS.H

GLOBALS.CPP


ADATA.
AADATA are some unit configuration files.
There is NEW operator overloaded, there is a object pool.

Utilities
===

FIXED.H
/*
**	This is a very simple fixed point class that functions like a regular integral type. However
**	it is under certain restrictions. The whole part must not exceed 65535. The fractional part is
**	limited to an accuracy of 1/65536. It cannot represent or properly handle negative values. It
**	really isn't all that fast (if an FPU is guaranteed to be present than using "float" might be
**	more efficient). It doesn't detect overflow or underflow in mathematical or bit-shift operations.
**
**	Take careful note that the normal mathematical operators return integers and not fixed point
**	values if either of the components is an integer. This is the normal C auto-upcasting rule
**	as it would apply presuming that integers are considered to be of higher precision than
**	fixed point numbers. This allows the result of these operators to generate values with greater
**	magnitude than is normally possible if the result were coerced into a fixed point number.
**	If the result should be fixed point, then ensure that both parameters are fixed point.
**
**	Note that although integers are used as the parameters in the mathematical operators, this
**	does not imply that negative parameters are supported. The use of integers is as a convenience
**	to the programmer -- constant integers are presumed signed. If unsigned parameters were
**	specified, then the compiler would have ambiguous conversion situation in the case of constant
** integers (e.g. 1, 10, 32, etc). This is most important for the constructor when dealing with the
**	"0" parameter case. In that situation the compiler might interpret the "0" as a null pointer rather
**	than an unsigned integer. There should be no adverse consequences of using signed integer parameters
**	since the precision/magnitude of these integers far exceeds the fixed point component counterparts.
**
**	Note that when integer values are returns from the arithmetic operators, the value is rounded
**	to the nearest whole integer value. This differs from normal integer math that always rounds down.
*/
class fixed

QUEUE.H
/*
**	This class implements a classic FIFO queue (also known as - standing in line). Objects
**	are added to the end (tail) of the line. Objects are removed from the start (first) of
**	the line. A keyboard buffer is a good example of a common computer use of a queue. There
**	is no provision for "taking cuts" or leaving the line once an object has been added.
**
**	The implementation uses a circular list of objects. This allows adding and deleting of
**	elements without any maintenance moves of remaining objects that would otherwise be
**	necessary in a simple array storage method. A side effect of this means that accessing the
**	internal array directly is not allowed. Supporting functions are provided for this purpose.
**
**	WARNING WARNING WARNING WARNING!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
**	The size parameter MUST be an exact power of two (2, 4, 8, 16, etc.) otherwise the internal
**	indexing algorithm will fail.
*/
template<class T, int size>
class QueueClass


HEAP.H
/**************************************************************************
**	This is a block memory managment handler. It is used when memory is to
**	be treated as a series of blocks of fixed size. This is similar to an
**	array of integral types, but unlike such an array, the memory blocks
**	are annonymous. This facilitates the use of this class when overloading
**	the new and delete operators for a normal class object.
*/
class FixedHeapClass


BASE.H
/****************************************************************************
** This is the class that defines a pre-built base for the computer AI.
** (Despite its name, this is NOT the "base" class for C&C's class hierarchy!)
*/
class BaseClass




Diagrams:
========

/***********************************************************************************************
 ***              C O N F I D E N T I A L  ---  W E S T W O O D  S T U D I O S               *** 
 ***********************************************************************************************
 *                                                                                             *
 *                 Project Name : Command & Conquer                                            *
 *                                                                                             *
 *                    File Name : GADGET.H                                                     *
 *                                                                                             *
 *                   Programmer : Maria del Mar McCready Legg                                  *
 *                                                                                             *
 *                   Start Date : January 3, 1995                                              *
 *                                                                                             *
 *                  Last Update : January 3, 1995   [MML]                                      *
 *                                                                                             *
 *                                                                                             *
 *        LinkClass [This is the linked list manager class. It keeps a record                  *
 *            ł      of the next and previous gadget in the list. It is possible               *
 *            ł      delete a gadget out of the middle of the list with this                   *
 *            ł      class.]                                                                   *
 *            ł                                                                                *
 *       GadgetClass [The is the basic gadget class. It handles processing of                  *
 *            ł       input events and dispatching the appropriate functions.                  *
 *            ł       All gadgets must be derived from this class.]                            *
 *            ĂÄÄÄÄż                                                                           *
 *            ł    ł                                                                           *
 *            ł  ListClass [Ths list class functions like a list box does in Windows. It       *
 *            ł             keeps track of a list of text strings. This list can be            *
 *            ł             scrolled and an item selected. If the list becomes larger than     *
 *            ł             can be completely displayed, it will automatically create a        *
 *            ł             slider (at the right edge) to manage the scrolling.]               *
 *            ł                                                                                *
 *      ControlClass [This class adds the concept of giving an ID number to the                *
 *            ł       gadget. This ID can then be returned from the Input()                    *
 *            ł       function as if it were a pseudo-keystroke. Additionally,                 *
 *            ł       the ability to inform another button that this button has                *
 *            ł       been actioned is allowed. This ability allows one button                 *
 *            ł       to watch what happens to another button. Example: a list                 *
 *            ł       box gadget can tell when an attached slider has been                     *
 *            ł       touched.]                                                                *
 *    ÚÄÄÄÄÄÄÄĹÄÄÄÄż                                                                           *
 *    ł       ł    ł                                                                           *
 *    ł       ł  GaugeClass [This class looks similar to Windows slider, but has               *
 *    ł       ł    ł         a different controlling logic. There is no thumb and              *
 *    ł       ł    ł         it serves as a simple variable control setting. This              *
 *    ł       ł    ł         is analagous to a volume slider.]                                 *
 *    ł       ł    ł                                                                           *
 *    ł       ł SliderClass [The slider class is similar to the typical Windows slider. It     *
 *    ł       ł              has a current setting, a thumb, and a controlable scale. This     *
 *    ł       ł              is the object created to handle a scrolling list box.]            *
 *    ł       ł                                                                                *
 *    ł   EditClass                                                                            *
 *    ł                                                                                        *
 *    ł                                                                                        *
 * ToggleClass [The toggle class is used for buttons that have an image and behave just        *
 *    ł         like the buttons in Windows do. That is, they have a separate visual for       *
 *    ł         when they are pressed and raised. They are officially triggered (return        *
 *    ł         their ID number) when the mouse button is released while over the button.      *
 *    ł         This class doesn't perform any rendering itself. It merely provides the        *
 *    ł         logic so that the derived classes will function correctly.]                    *
 *  ÚÄÁÄÄÄÄż                                                                                   *
 *  ł      ł                                                                                   *
 *  ł   TextButtonClass [The text button functions like a normal Windows style button, but     *
 *  ł                    the imagery is based on text that is displayed on the button. A       *
 *  ł                    typical example would be the "OK" or "Cancel" buttons.]               *
 *  ł                                                                                          *
 * ShapeButtonClass [The shape buttons is similar to the TextButton but instead of text        *
 *                   being used to give the button its imagery, an actual shape is used        *
 *                   instead. This allows graphic buttons. These are similar to the up/down    *
 *                   arrows seen in a Windows slider.]                                         *
 *                                                                                             *
 * - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - */

 /*
 Map (screen) class heirarchy.

 MapeditClass (most derived class) -- scenario editor
        ł
   MouseClass -- handles mouse animation and display control
        ł
  ScrollClass -- map scroll handler
        ł
    HelpClass -- pop-up help text handler
        ł
     TabClass -- file folder tab screen mode control dispatcher
        ł
 SidebarClass -- displays and controls construction list sidebar
        ł
   PowerClass -- display power production/consumption bargraph
        ł
   RadarClass -- displays and controls radar map
        ł
 DisplayClass -- general tactical map display handler
        ł
     MapClass -- general tactical map data handler
        ł
 GScreenClass (pure virtual base class) -- generic screen control



                            AbstractClass
                                  ł
                                  ł
                                  ł
                                  ł
                            ObjectClass
                                  ł
       ÚÄÄÄÄÄÄÂÄÄÄÄÄÄÄÄÄÄÂÄÄÄÄÄÄÄÄĹÄÄÄÄÄÄÄÄÂÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÂÄÄÄÄÄÄÄÄÄÄÄż
   AnimClass  ł  TemplateClass    ł        ĂÄ FuseClass     ł    TerrainClass
              ł                   ł        ĂÄ FlyClass      ł
              ł                   ł  BulletClass            ł
       OverlayClass        MissionClass               SmudgeClass
                                  ł
                             RadioClass
                                  ł
                                  ĂÄ CrewClass
                                  ĂÄ FlasherClass
                                  ĂÄ StageClass
                                  ĂÄ CargoClass
                            TechnoClass
                                  ł
         ÚÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÁÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄż
     FootClass                                         BuildingClass
         ł
         ĂÄÄÄÄÄÄÄÄÄÄÄÄÄÄÂÄÄÄÄÄÄÄÄÄÄÄÄÄż
    DriveClass  InfantryClass         ĂÄ FlyClass
         ł                      AircraftClass
   TurretClass
         ł
   TarComClass
         ł
     UnitClass


                            AbstractTypeClass
                                    ł
                              ObjectTypeClass
                                    ł
             ÚÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄĹÄÄÄÄÄÄÄÄÄÄÄÄÂÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄÄż
             ł                      ł            ł                 ł
       TechnoTypeClass              ł            ł                 ł
             ł                BulletTypeClass    ł                 ł
             ł                           TemplateTypeClass         ł
    ÚÄÄÄÄÄÄÄÄÁÄÄÄÄÄÂÄÄÄÄÄÄÄÄÄÄÄÂÄÄÄÄÄÄÄÄÄÄÄÄÄÄż             TerrainTypeClass
    ł              ł           ł              ł
UnitTypeClass      ł   BuildingTypeClass      ł
                   ł                  InfantryTypeClass
           AircraftTypeClass
*/

