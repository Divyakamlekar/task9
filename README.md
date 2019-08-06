namespace MyTested.AspNetCore.Mvc
{
    using System;
    using Builders.ActionResults.Created;
    using Builders.Base;
    using Builders.Contracts.ActionResults.Created;
    using Builders.Contracts.Uri;
    using Exceptions;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.AspNetCore.Mvc.Formatters;
    using Utilities.Validators;

    /// <summary>
    /// Contains extension methods for <see cref="ICreatedTestBuilder"/>.
    /// </summary>
    public static class CreatedTestBuilderExtensions
    {
        private const string Location = "location";

        /// <summary>
        /// Tests whether the <see cref="CreatedResult"/>
        /// has specific location provided by string.
        /// </summary>
        /// <param name="createdTestBuilder">
        /// Instance of <see cref="ICreatedTestBuilder"/> type.
        /// </param>
        /// <param name="location">Expected location as string.</param>
        /// <returns>The same <see cref="IAndCreatedTestBuilder"/>.</returns>
        public static IAndCreatedTestBuilder AtLocation(
            this ICreatedTestBuilder createdTestBuilder,
            string location)
        {
            var actualBuilder = GetCreatedTestBuilder<CreatedResult>(createdTestBuilder, Location);

            var uri = LocationValidator.ValidateAndGetWellFormedUriString(
                location, 
                actualBuilder.ThrowNewFailedValidationException);

            return actualBuilder.AtLocation(uri);
        }

        /// <summary>
        /// Tests whether the <see cref="CreatedResult"/>
        /// location passes the given assertions.
        /// </summary>
        /// <param name="createdTestBuilder">
        /// Instance of <see cref="ICreatedTestBuilder"/> type.
        /// </param>
        /// <param name="assertions">Action containing all assertions for the location.</param>
        /// <returns>The same <see cref="IAndCreatedTestBuilder"/>.</returns>
        public static IAndCreatedTestBuilder AtLocationPassing(
            this ICreatedTestBuilder createdTestBuilder, 
            Action<string> assertions)
        {
            var actualBuilder = GetCreatedTestBuilder<CreatedResult>(createdTestBuilder, Location);
            
            assertions(actualBuilder.ActionResult.Location);

            return actualBuilder;
        }

        /// <summary>
        /// Tests whether the <see cref="CreatedResult"/>
        /// location passes the given predicate.
        /// </summary>
        /// <param name="createdTestBuilder">
        /// Instance of <see cref="ICreatedTestBuilder"/> type.
        /// </param>
        /// <param name="predicate">Predicate testing the location.</param>
        /// <returns>The same <see cref="IAndCreatedTestBuilder"/>.</returns>
        public static IAndCreatedTestBuilder AtLocationPassing(
            this ICreatedTestBuilder createdTestBuilder, 
            Func<string, bool> predicate)
        {
            var actualBuilder = GetCreatedTestBuilder<CreatedResult>(createdTestBuilder, Location);
            
            var location = actualBuilder.ActionResult.Location;

            if (!predicate(location))
            {
                actualBuilder.ThrowNewFailedValidationException(
                    $"location ('{location}')",
                    "to pass the given predicate",
                    "it failed");
            }

            return actualBuilder;
        }

        /// <summary>
        /// Tests whether the <see cref="CreatedResult"/>
        /// has specific location provided by <see cref="Uri"/>.
        /// </summary>
        /// <param name="createdTestBuilder">
        /// Instance of <see cref="ICreatedTestBuilder"/> type.
        /// </param>
        /// <param name="location">Expected location as <see cref="Uri"/>.</param>
        /// <returns>The same <see cref="IAndCreatedTestBuilder"/>.</returns>
        public static IAndCreatedTestBuilder AtLocation(
            this ICreatedTestBuilder createdTestBuilder, 
            Uri location)
        {
            var actualBuilder = GetCreatedTestBuilder<CreatedResult>(createdTestBuilder, Location);

            LocationValidator.ValidateUri(
                actualBuilder.ActionResult,
                location.OriginalString,
                actualBuilder.ThrowNewFailedValidationException);

            return actualBuilder;
        }

        /// <summary>
        /// Tests whether the <see cref="CreatedResult"/>
        /// has specific location provided by builder.
        /// </summary>
        /// <param name="createdTestBuilder">
        /// Instance of <see cref="ICreatedTestBuilder"/> type.
        /// </param>
        /// <param name="uriTestBuilder">Builder for expected URI.</param>
        /// <returns>The same <see cref="IAndCreatedTestBuilder"/>.</returns>
        public static IAndCreatedTestBuilder AtLocation(
            this ICreatedTestBuilder createdTestBuilder, 
            Action<IUriTestBuilder> uriTestBuilder)
        {
            var actualBuilder = GetCreatedTestBuilder<CreatedResult>(createdTestBuilder, Location);

            LocationValidator.ValidateLocation(
                actualBuilder.ActionResult,
                uriTestBuilder,
                actualBuilder.ThrowNewFailedValidationException);

            return actualBuilder;
        }

        /// <summary>
        /// Tests whether the <see cref="CreatedAtActionResult"/>
        /// has specific action name.
        /// </summary>
        /// <param name="createdTestBuilder">
        /// Instance of <see cref="ICreatedTestBuilder"/> type.
        /// </param>
        /// <param name="actionName">Expected action name.</param>
        /// <returns>The same <see cref="IAndCreatedTestBuilder"/>.</returns>
        public static IAndCreatedTestBuilder AtAction(
            this ICreatedTestBuilder createdTestBuilder, 
            string actionName)
        {
            var actualBuilder = GetCreatedTestBuilder<CreatedAtActionResult>(
                createdTestBuilder,
                "action name");
            
            RouteActionResultValidator.ValidateActionName(
                actualBuilder.ActionResult,
                actionName,
                actualBuilder.ThrowNewFailedValidationException);

            return actualBuilder;
        }

        /// <summary>
        /// Tests whether the <see cref="CreatedAtActionResult"/>
        /// has specific controller name.
        /// </summary>
        /// <param name="createdTestBuilder">
        /// Instance of <see cref="ICreatedTestBuilder"/> type.
        /// </param>
        /// <param name="controllerName">Expected controller name.</param>
        /// <returns>The same <see cref="IAndCreatedTestBuilder"/>.</returns>
        public static IAndCreatedTestBuilder AtController(
            this ICreatedTestBuilder createdTestBuilder, 
            string controllerName)
        {
            var actualBuilder = GetCreatedTestBuilder<CreatedAtActionResult>(
                createdTestBuilder,
                "controller name");
            
            RouteActionResultValidator.ValidateControllerName(
                actualBuilder.ActionResult,
                controllerName,
                actualBuilder.ThrowNewFailedValidationException);

            return actualBuilder;
        }

        /// <summary>
        /// Tests whether the <see cref="CreatedAtRouteResult"/>
        /// has specific route name.
        /// </summary>
        /// <param name="createdTestBuilder">
        /// Instance of <see cref="ICreatedTestBuilder"/> type.
        /// </param>
        /// <param name="routeName">Expected route name.</param>
        /// <returns>The same <see cref="IAndCreatedTestBuilder"/>.</returns>
        public static IAndCreatedTestBuilder AtRoute(
            this ICreatedTestBuilder createdTestBuilder,
            string routeName)
            => createdTestBuilder
                .WithRouteName(routeName);

        /// <summary>
        /// Tests whether the created result
        /// contains <see cref="IOutputFormatter"/> of the provided type.
        /// </summary>
        /// <param name="createdTestBuilder">
        /// Instance of <see cref="ICreatedTestBuilder"/> type.
        /// </param>
        /// <returns>The same <see cref="IAndCreatedTestBuilder"/>.</returns>
        public static IAndCreatedTestBuilder ContainingOutputFormatterOfType<TOutputFormatter>(
            this ICreatedTestBuilder createdTestBuilder)
            where TOutputFormatter : IOutputFormatter
            => createdTestBuilder
                .ContainingOutputFormatterOfType<IAndCreatedTestBuilder, TOutputFormatter>();

        /// <summary>
        /// Tests whether the <see cref="CreatedAtActionResult"/> or <see cref="CreatedAtRouteResult"/>
        /// contains specific route value of the given type.
        /// </summary>
        /// <param name="createdTestBuilder">
        /// Instance of <see cref="ICreatedTestBuilder"/> type.
        /// </param>
        /// <returns>The same <see cref="IAndCreatedTestBuilder"/>.</returns>
        public static IAndCreatedTestBuilder ContainingRouteValueOfType<TRouteValue>(
            this ICreatedTestBuilder createdTestBuilder)
            => createdTestBuilder
                .ContainingRouteValueOfType<IAndCreatedTestBuilder, TRouteValue>();

        /// <summary>
        /// Tests whether the <see cref="CreatedAtActionResult"/> or <see cref="CreatedAtRouteResult"/>
        /// contains specific route value of the given type with the provided key.
        /// </summary>
        /// <param name="createdTestBuilder">
        /// Instance of <see cref="ICreatedTestBuilder"/> type.
        /// </param>
        /// <param name="key">Expected route key.</param>
        /// <returns>The same <see cref="IAndCreatedTestBuilder"/>.</returns>
        public static IAndCreatedTestBuilder ContainingRouteValueOfType<TRouteValue>(
            this ICreatedTestBuilder createdTestBuilder,
            string key)
            => createdTestBuilder
                .ContainingRouteValueOfType<IAndCreatedTestBuilder, TRouteValue>(key);

        /// <summary>
        /// Tests whether the <see cref="CreatedAtActionResult"/> or <see cref="CreatedAtRouteResult"/>
        /// has the same <see cref="IUrlHelper"/> type as the provided one.
        /// </summary>
        /// <param name="createdTestBuilder">
        /// Instance of <see cref="ICreatedTestBuilder"/> type.
        /// </param>
        /// <returns>The same <see cref="IAndCreatedTestBuilder"/>.</returns>
        public static IAndCreatedTestBuilder WithUrlHelperOfType<TUrlHelper>(
            this ICreatedTestBuilder createdTestBuilder)
            where TUrlHelper : IUrlHelper
            => createdTestBuilder
                .WithUrlHelperOfType<IAndCreatedTestBuilder, TUrlHelper>();

        private static CreatedTestBuilder<TCreatedResult> GetCreatedTestBuilder<TCreatedResult>(
            ICreatedTestBuilder createdTestBuilder,
            string containment)
            where TCreatedResult : ObjectResult
        {
            var actualCreatedTestBuilder = createdTestBuilder as CreatedTestBuilder<TCreatedResult>;

            if (actualCreatedTestBuilder == null)
            {
                var createdTestBuilderBase = (BaseTestBuilderWithComponent)createdTestBuilder;

                throw new CreatedResultAssertionException(string.Format(
                    "{0} created result to contain {1}, but such could not be found.",
                    createdTestBuilderBase.TestContext.ExceptionMessagePrefix,
                    containment));
            }

            return actualCreatedTestBuilder;
            
             public static class FileTestBuilderExtensions
    {
        private const string FileStream = "stream";
        private const string FileName = "name";
        private const string FileProvider = "provider";
        private const string FileContents = "contents";

        /// <summary>
        /// Tests whether the <see cref="FileStreamResult"/>
        /// has the same file stream as the provided one.
        /// </summary>
        /// <param name="fileTestBuilder">
        /// Instance of <see cref="IFileTestBuilder"/> type.
        /// </param>
        /// <param name="stream">File stream.</param>
        /// <returns>The same <see cref="IAndFileTestBuilder"/>.</returns>
        public static IAndFileTestBuilder WithStream(
            this IFileTestBuilder fileTestBuilder,
            Stream stream)
        {
            var actualBuilder = GetFileTestBuilder<FileStreamResult>(fileTestBuilder, FileStream);

            var fileStreamResult = actualBuilder.ActionResult;
            var expectedContents = stream.ToByteArray();
            var actualContents = fileStreamResult.FileStream.ToByteArray();

            if (!expectedContents.SequenceEqual(actualContents))
            {
                actualBuilder.ThrowNewFailedValidationException(
                    FileStream,
                    "to have value as the provided one",
                    "instead received different result");
            }

            return actualBuilder;
        }

        /// <summary>
        /// Tests whether the <see cref="VirtualFileResult"/>
        /// has the same file name as the provided one.
        /// </summary>
        /// <param name="fileTestBuilder">
        /// Instance of <see cref="IFileTestBuilder"/> type.
        /// </param>
        /// <param name="fileName">File name as string.</param>
        /// <returns>The same <see cref="IAndFileTestBuilder"/>.</returns>
        public static IAndFileTestBuilder WithName(
            this IFileTestBuilder fileTestBuilder,
            string fileName)
        {
            var actualBuilder = GetFileTestBuilder<VirtualFileResult>(fileTestBuilder, FileName);
            
            var virtualFileResult = actualBuilder.ActionResult;
            var actualFileName = virtualFileResult.FileName;

            if (fileName != virtualFileResult.FileName)
            {
                actualBuilder.ThrowNewFailedValidationException(
                    FileName,
                    $"to be '{fileName}'",
                    $"instead received '{actualFileName}'");
            }

            return actualBuilder;
        }

        /// <summary>
        /// Tests whether the <see cref="VirtualFileResult"/>
        /// has the same file provider as the provided one.
        /// </summary>
        /// <param name="fileTestBuilder">
        /// Instance of <see cref="IFileTestBuilder"/> type.
        /// </param>
        /// <param name="fileProvider">File provider of type <see cref="IFileProvider"/>.</param>
        /// <returns>The same <see cref="IAndFileTestBuilder"/>.</returns>
        public static IAndFileTestBuilder WithFileProvider(
            this IFileTestBuilder fileTestBuilder,
            IFileProvider fileProvider)
        {
            var actualBuilder = GetFileTestBuilder<VirtualFileResult>(fileTestBuilder, FileProvider);

            var virtualFileResult = actualBuilder.ActionResult;

            if (fileProvider != virtualFileResult.FileProvider)
            {
                actualBuilder.ThrowNewFailedValidationException(
                    FileProvider,
                    "to be the same as the provided one",
                    "instead received different result");
            }

            return actualBuilder;
        }

        /// <summary>
        /// Tests whether the <see cref="VirtualFileResult"/>
        /// has the same file provider type as the provided one.
        /// </summary>
        /// <param name="fileTestBuilder">
        /// Instance of <see cref="IFileTestBuilder"/> type.
        /// </param>
        /// <returns>The same <see cref="IAndFileTestBuilder"/>.</returns>
        public static IAndFileTestBuilder WithFileProviderOfType<TFileProvider>(
            this IFileTestBuilder fileTestBuilder)
            where TFileProvider : IFileProvider
        {
            var actualBuilder = GetFileTestBuilder<VirtualFileResult>(fileTestBuilder, FileProvider);

            var virtualFileResult = actualBuilder.ActionResult;
            var actualFileProvider = virtualFileResult.FileProvider;

            if (actualFileProvider == null ||
                Reflection.AreDifferentTypes(typeof(TFileProvider), actualFileProvider.GetType()))
            {
                actualBuilder.ThrowNewFailedValidationException(
                    FileProvider,
                    $"to be of {typeof(TFileProvider).Name} type",
                    $"instead received {actualFileProvider.GetName()}");
            }

            return actualBuilder;
        }

        /// <summary>
        /// Tests whether the <see cref="FileContentResult"/>
        /// has the same file contents as the provided byte array.
        /// </summary>
        /// <param name="fileTestBuilder">
        /// Instance of <see cref="IFileTestBuilder"/> type.
        /// </param>
        /// <param name="fileContents">File contents as byte array.</param>
        /// <returns>The same <see cref="IAndFileTestBuilder"/>.</returns>
        public static IAndFileTestBuilder WithContents(
            this IFileTestBuilder fileTestBuilder,
            byte[] fileContents)
        {
            var actualBuilder = GetFileTestBuilder<FileContentResult>(fileTestBuilder, FileContents);

            var fileContentResult = actualBuilder.ActionResult;

            if (!fileContents.SequenceEqual(fileContentResult.FileContents))
            {
                actualBuilder.ThrowNewFailedValidationException(
                    FileContents,
                    "to have values as the provided ones",
                    "instead received different result");
            }

            return actualBuilder;
        }
        
        private static FileTestBuilder<TFileResult> GetFileTestBuilder<TFileResult>(
            IFileTestBuilder fileTestBuilder,
            string containment)
            where TFileResult : FileResult
        {
            var actualFileTestBuilder = fileTestBuilder as FileTestBuilder<TFileResult>;

            if (actualFileTestBuilder == null)
            {
                var fileTestBuilderBase = (BaseTestBuilderWithComponent)fileTestBuilder;

                throw new FileResultAssertionException(string.Format(
                    "{0} file result to contain {1}, but such could not be found.",
                    fileTestBuilderBase.TestContext.ExceptionMessagePrefix,
                    containment));
            }

            return actualFileTestBuilder;
        }
    }
}
