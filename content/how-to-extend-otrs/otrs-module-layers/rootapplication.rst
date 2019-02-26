Root Application Module Layer
=============================

The root application module layer enables the developers to develop and load custom front end components. Components will be lazy loaded and are omnipresent in the front end application. They can use any API that is exposed to other components within the application, including its state.

Developing a custom root application component is as easy as creating a new file.


Root Application Component Code Example
---------------------------------------

In this small example we'll write a little component, which outputs a custom footer on every page of the agent interface app.

Just create a new file ``SampleFooter.vue`` in ``/Frontend/Apps/Agent/Components/RootApplicationModule/Modules`` folder.

.. code-block:: HTML

   <template>
       <div class="CustomFooter">This is my custom footer.</div>
   </template>

   <script>
   export default {
       name: 'SampleFooter',
   };
   </script>

   <style lang="scss">
   .CustomFooter {
       width: 100%;
       height: 30px;
       border-top: 1px solid #000;
       padding: 2px;
       text-align: center;
   }
   </style>

To activate our custom footer module, just rebuild the app that was modified. If a development server is used, it will need to be restarted to pick up the changes.
